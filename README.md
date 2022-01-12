<p align="center">
    <img src="https://raw.githubusercontent.com/octree-gva/meta/main/decidim/static/octree_and_decidim.png" height="90" alt="Decidim Research & Development by Octree" />
</p>
<h3 align="center">
    <strong>Decidim In Docker</strong><br />
    A Proof of Concept for using Decidim in Docker from freshly generated app.<br />
</h3>
<h4 align="center">
    <a href="https://github.com/decidim/decidim/issues/8517">Issue</a> |
    <a href="https://meta.decidim.org/processes/roadmap/f/122/proposals/16846">Meta decidim proposal</a>
</h4><br /><br />

# Run the POC

## 1. Prepare Data

```bash
docker-compose run --rm app rails db:migrate
docker-compose run --rm app rails c
$ (email, password)=["john@doe.com", "secure-password"]
$ Decidim::System::Admin.create!(email: email, password: password, password_confirmation: password)
```

## 2. Set Credentials
Environnement `RAILS_MASTER_KEY` is a critical value, and should never be in a git file. To be secure by default, you need to setup a new master key:

```bash
# remove encrypted files
rm -f config/credentials.yml.enc config/master.key
# set up new credentials
docker-compose run --rm app bash -c 'EDITOR="vim" bin/rails credentials:edit'
# type ":wq" and press enter to save and quit the vim editor (you can leave defaults)
```

Once you have your master key configured, add it to your deployment manually and **never commit this file**

## 3. Run the app
```bash
docker-compose up
# Access localhost:3000 (The rails app)
```


<br /><br />

# How it works
In the docker-compose, you can see: 

* (public net) a NGinx that serve the rails app and assets
    * (private net) A rails app that serve the decidim app
    * (private net) A postgres database

The aims of this docker-compose is to showcase how can we deploy securely 
an application for production, with a rails app and database in a private network.


<br /><br />

# License
<img src="https://raw.githubusercontent.com/octree-gva/meta/main/decidim/static/decidim_licence.png" width="120"><br /><br />
This repository is released under [AGPL-V3](https://choosealicense.com/licenses/agpl-3.0/). 

<br /><br /><br /><br />

# Decidim?
[Decidim](https://github.com/decidim/decidim) is a participatory democracy framework, written in Ruby on Rails, originally developed for the Barcelona City government online and offline participation website.
<br />
<h4>
    <a href="https://decidim.org">Decidim Website</a> |
    <a href="https://docs.decidim.org/en/">Decidim Docs</a> |
    <a href="https://meta.decidim.org">Participatory Governance (meta decidim)</a><br/>
    <a href="https://matrix.to/#/+decidim:matrix.org">Decidim Community (Matrix+Element.io)</a>
</h4>


