# NanoMine
NanoMine Nanocomposites Data Resource

##### NOTE: Prototype code. Some approaches will change. Monitor this README for updates.

# Installation 
#### (REQUIRES ubuntu 16.04 -- 18.04 will not work yet)
### Note: if installing on a virtual machine, be sure to allocate at least 8G Memory, 2 CPUs and 30G Disk
- install [whyis](http://tetherless-world.github.io/whyis/install) using this command (bluedevil-oit version contains proxy work-around)
  ```
  bash < <(curl -skL https://raw.githubusercontent.com/bluedevil-oit/whyis/release/install.sh) # master does not seem to ingest properly
  ```
- whyis will be installed in /apps/whyis
- Steps to install NanoMine:
  ```
  cd /apps
  sudo git clone https://github.com/YOURFORK/nanomine.git  # to use the original, use FORKNAME of 'duke-matsci'
  sudo mkdir -p /apps/nanomine/data 2>/dev/null
  cd /apps/nanomine/data
  sudo wget https://raw.githubusercontent.com/duke-matsci/nanomine-ontology/master/xml_ingest.setl.ttl
  sudo wget https://raw.githubusercontent.com/duke-matsci/nanomine-ontology/master/ontology.setl.ttl
  sudo chown -R whyis:whyis /apps/nanomine
  sudo su - whyis
  
  #install n - the nodejs version manager and LTS version of node
  curl -L https://git.io/n-install | bash -s -- -y lts
  echo 'export N_PREFIX="$HOME/n"; [[ :$PATH: == *":$N_PREFIX/bin:"* ]] || PATH+=":$N_PREFIX/bin"' >> ~/.bash_profile
  
  #make sure n is in the path
  source ~/.bash_profile

  # install the VueJS command line processor
  npm i -g vue-cli@2.9.6  
  
  cd /apps/nanomine

  # create node_modules directory and install required components
  npm install
  
  # build the vue application
  #  (do not forget this -- results in 'Server Error' in browser otherwise)
  #    Also, if running the GUI under apache/whyis, the build will need to be re-run
  #    if gui changes are made.
  npm run build
  
  #install NanoMine python components for Whyis
  pip install -e .
  exit
  
  sudo a2enmod proxy_connect.load  
  sudo a2enmod proxy_html.load  
  sudo a2enmod proxy_http.load  
  sudo a2enmod proxy.load

  sudo cp /apps/nanomine/install/000-default.conf /etc/apache2/sites-available
  
  
  sudo service apache2 restart
  sudo service celeryd restart
  sudo su - whyis
  
  cd nanomine/rest
  npm i
  # the next command will run the rest server in the background
  #   If you're testing the rest server, it might be a good idea to 
  #   leave off the ampersand and run 'node index.js'
  #   in a new console window by itself so that starting/stopping will
  #   be easier (ctrl+c).  We will make this into a system service soon.
  node index.js &
  
  cd /apps/whyis 
   
  python manage.py createuser -e (email) -p (password) -f (firstname) -l (lastname) -u (username) --roles=admin
  # NOTE: if messages like this are seen: 'numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88'
  #         ref: https://stackoverflow.com/questions/25752444/scipy-error-numpy-dtype-size-changed-may-indicate-binary-incompatibility-and
  #       then, try these commands:
  #    pip uninstall -y scipy numpy pandas
  #    pip install scipy==1.1.0  numpy==1.14.5  pandas==0.23.1
  
  python manage.py load -i /apps/nanomine/nm.ttl -f turtle
  cd /apps
  touch .netrc
  ```
### OK to skip the .netrc edit and load phases at least for now...
- after you create the .netrc file under /apps, edit the file and add the following.

  ```
  machine some_url (like 000.000.000.000) NO PORT -- this is the address of the nanomine MDCS server
  login some_username (like testuser1)
  password some_password (like testpwd1)
  ```
- after you edit the .netrc file, in your terminal type:
  - the xml_ingest.setl.ttl file references the nanomine server protocol, address and port and may need to  be edited
  ```
  cd /apps/whyis
  python manage.py load -i /apps/nanomine/data/ontology.setl.ttl -f turtle
  python manage.py load -i /apps/nanomine/data/xml_ingest.setl.ttl -f turtle
  ```
  - The load process can be monitored with 'sudo tail -f /var/log/celery/w1.log'
  
### Use the server...  
- go to http://localhost/ to login with your credentials during "createuser" command
- go to http://localhost/nm to access NanoMine

# Server Development mode
Each time a change is made for NanoMine, apache2 and celeryd service have to be restarted manually. 
It is possible to use Whyis' development mode to bypass the need to restart services.

Try this:  
```
sudo su - whyis
cd /app/whyis
python manage.py runserver -h 0.0.0.0
``` 
After executing the commands above NanoMine may accessed on "http://localhost:5000/nm".
Then, you only need to refresh your webpage to see your changes immediately after you make changes to NanoMine. 

After you finished the visualization changes, you can shutdown the developing mode with CTRL+c.
Then you have to restart apache2 and celeryd service by
```
sudo service apache2 restart
sudo service celeryd restart
```
After this, the updated NanoMine app will show up at "http://localhost/nm".

# Components and Libraries Used
- https://vuejs.org VueJS Javascript Framework
- https://vuejs.org/v2/guide/ VueJS Introduction
- https://router.vuejs.org/. Internal Router
- https://vue-loader.vuejs.org/. VueJS webpack component loader (Build)
- https://vuex.vuejs.org/ VueJS global state management
- https://vuetifyjs.com/en/ Vuetify Material Design CSS framework
  - NOTE: based on Material Design - https://material.io 
- https://vuetifyjs.com/en/getting-started/quick-start Vuetify CSS framework docs
- https://github.com/axios/axios Axios Remote Request Library
- https://www.npmjs.com/package/axios Axios NPM package info
- https://google.github.io/material-design-icons/ ICONS 
- https://expressjs.com ExpressJS 


# Development
Fork the nanomine repository and use the 'dev' branch for 
development (git checkout dev). Ensure 
that pull requests are for the dev branch and not master and not QA.

NOTE: The install instructions above clone the repo from the duke-matsci
using https. Additionally, the files become owned by the 'whyis' user.
This may make it a bit difficult for on-going development. The best work-around
for now is probably to fork the repo and use git ssh under your own user.
However, this will complicate runtime. The readme will be updated with better
instructions once a good alternative is worked out.


```
Since NanoMine's install is similar to the NanomineViz 
install, the instructions for NanoMine (above) are based on a modified 
version of Rui's NanomineViz instructions from raymondino/NanomineViz
```
