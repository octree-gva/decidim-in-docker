# Decidim In Docker

This is a Proof of Concept for using Docker for decidim installation.

## Getting started

1. Prepare Data
```
docker-compose run --rm app rails db:migrate
docker-compose run --rm app rails c
$ (email, password)=["john@doe.com", "secure-password"]
$ Decidim::System::Admin.create!(email: email, password: password, password_confirmation: password)
```

2. Run the app
```
docker-compose up
# Access the app in localhost:8080 (Nginx)
# Rails run under a PRIVATE port 3000
```
In the docker-compose, you can see: 

* (public net) a NGinx that serve the rails app and assets
    * (private net) A rails app that serve the decidim app
    * (private net) A posgres database


# TODOS

- [ ] Configuring environments for secrets and 12-factors good practices
- [ ] Steps docker build for testing (chromium and stuff)
- [ ] Analyse launcher from discuss and see feasibility


##
This is the open-source repository for decidim_in_docker, based on [Decidim](https://github.com/decidim/decidim).

## Setting up the application

You will need to do some steps before having the app working properly once you've deployed it:

1. Open a Rails console in the server: `bundle exec rails console`
2. Create a System Admin user:
```ruby
user = Decidim::System::Admin.new(email: <email>, password: <password>, password_confirmation: <password>)
user.save!
```
3. Visit `<your app url>/system` and login with your system admin credentials
4. Create a new organization. Check the locales you want to use for that organization, and select a default locale.
5. Set the correct default host for the organization, otherwise the app will not work properly. Note that you need to include any subdomain you might be using.
6. Fill the rest of the form and submit it.

You're good to go!
