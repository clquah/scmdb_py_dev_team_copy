# scmdb_py

## Development Setup
1. Clone the git repo.

   * `git clone https://github.com/mukamel-lab-team/scmdb_py_dev.git; cd scmdb_py_dev`

2. Set up virtual environment. 

   * `virtualenv venv -p python3`
   * `source venv/bin/activate`
   * `pip install --upgrade pip`
   * `pip install -r scmdb_py/requirements.txt`
   * `deactivate`

3. Set up default_config.py.
   * *Make sure to not push default_config.py to github unless you have removed  
all sensitive information!*
4. Run.
   * `bash run_dev.sh` *May need to change ports*
5. On your web browser, go to `localhost:<port>`
6. If running from a remote host (ie Banjo, Brainome), you must create an ssh tunnel  
from your local machine.
   * `ssh -NfL localhost:<port>:localhost:<port> <user_name>@<server_name>`


## Directory structure
```
scmdb_py_dev/
|-- scmdb_py.wsgi                           *WSGI script file for hosting via apache *Don't Touch*
|-- requirements.txt                        *list of required python packages
|-- run_dev.sh
|-- scmdb_py/
|   |-- __init__.py                         *Application factory (setup)
|   |-- frontend.py                         *responsible for all views (handles URL requests)
|   |-- content.py                          *all server side data querying and plot generation
|   |-- assets.py                           *gathers all javascript files in assets directory
|   |-- default_config.py                   *Configuration file for Flask. (info for MySQL, email, etc.)
|   |-- assets/                             *All your .js and .css files go here
|   |   |-- scripts/                        *Individual javascript files
|   |   |   |-- app.js                      
|   |   |   |-- customview.js               *Frontend functions for data browser (plotting, sending queries)
|   |   |   |-- request_new_ensemble.js     *Frontend functions for request new ensemble page
|   |   |   |-- tabular_dataset_rs1.js      *Frontend functions for summary (metadata) pages
|   |   |   |-- tabular_dataset_rs2.js
|   |   |   |-- tabular_ensemble.js
|   |   |   |-- vendor/                 *All downloaded javascript libraries
|   |   |   |   |-- ...
|   |   |-- styles/                         *Individual css files
|   |   |   |-- app_base.css                *Style for navigation bar.
|   |   |   |-- browser.css                 *Style for data browser
|   |   |   |-- vendor/                 *All downloaded css files
|   |   |   |   |-- ...
|   |-- templates/                          *HTML files 
|   |   |-- base.html                       *Base template file for ensembleview.html
|   |   |-- ensembleview.html               *Main HTML file for data browser page of portal
|   |   |-- request_new_ensemble.html
|   |   |-- tabular_dataset_rs1.html
|   |   |-- tabular_dataset_rs2.html
|   |   |-- tabular_ensemble.html
|   |   |-- accounts/                       *HTML files for account/login pages
|   |   |-- components/                     *Individual componenents of ensembleview.html
|   |   |   |-- search_options.html         *Gene methylation search box
|   |   |   |-- mch_box.html                *Methylation box plot / heatmap
|   |   |   |-- mch_correlated_genes.html   *Methylation box plot / heatmap
|   |   |   |-- mch_scatter.html            *Methylation cluster plot
|   |   |-- partials/                       *HTML code to be included in all pages
|   |   |   |-- _flashes.html               *flash message settings
|   |   |   |-- _head.html                  *included in <head> of all pages
|   |   |-- macros/                         *Macros (functions) used by jinja to generate HTML
|   |   |   |-- nav_macros.html             *Edit this to change navigation bar items
|   |-- static/                             *All static files (.js files, images) *Don't Touch*
```

## How requests are handled
1. User clicks a button on page. (ie. Search for the gene Sox6)
2. Appropriate javascript function sends an AJAX request via HTML GET or POST method.
3. frontend.py handles the URL route and uses content.py to gather data / generate plots.
4. frontend.py sends the JSON formatted data back to the client.
5. Javascript updates what the user sees on screen. 
