ferris-users
============

    Note: This plugin is new, so there are still some minor issues to iron out.

A plugin for the Ferris Framework allowing custom user accounts and social login.

The code was taken from [GAE Boilerplate](https://github.com/coto/gae-boilerplate) and adapted as a plugin for the **Ferris Framework**.

###Features

This plugin provides the following features:

- Create an account with your email address
- Login with *Google*, *Github*, *Twitter*, *Facebook*, *Yahoo* or *LinkedIn*
- Email activation
- Reset Password

###Dependencies

This plugin has some dependencies for certain functionality. Install the following plugins in your app:

- [ferris-recaptcha](https://github.com/robertdodd/ferris-recaptcha) - Uses this captcha plugin to protect against password-reset spamming.
- [ferris-csrf](https://github.com/robertdodd/ferris-csrf) - Uses this plugin to protect against cross-site request forgery.

###Get started
If you have not done so already, [Download the Ferris Framework](https://bitbucket.org/cloudsherpas/ferris-framework/get/master.zip) and read the [Getting Started](http://ferris-framework.appspot.com/docs/getting_started.html) guide.

####1. Install the plugin

Copy the `custom_auth` folder from this repo into the `plugins` directory of your ferris app.

Also install the plugins listed under **Dependencies** above.

####2. Enable the plugin

Add the following line to your `app/routes.py` file:

`plugins.enable('custom_auth')`

Also enable the plugins listed under **Dependencies**.

####3. Setup the plugin

You need to add some custom settings to your app.

Copy the contents of `custom_auth/settings.py` to the bottom of `app/settings.py`.

Because this plugin sends emails, you also need to set the `sender` address in `settings['email']`, or else Ferris won't send emails.

This can be the email address of a registered admin, your apps address `xxx@{YOUR_APP_ID}.appspotmail.com` or a domain address you have associated with your account.

####4. Using the plugin

The following routes are automatically available:

- Login: `/login`
- Register: `/register`
- Logout: `/logout`
- Edit Profile: `/user/edit`

####5. Using the components:

You can enable csrf protection in a controller like this:

```
from ferris import Model, Controller, scaffold
from plugins.custom_auth.components import custom_auth

class Posts(Controller):
    class Meta:
        components = (scaffold.Scaffolding, custom_auth.CustomAuth,)
    ...
```

The following [authorizations](http://ferris-framework.appspot.com/docs/users_guide/authorization_chains.html) are available:

- `custom_auth.require_user`: Requires that a user is logged in
- `custom_auth.require_not_user`: Requires that a user is NOT logged in

The following variables are automatically available in templates:

- `user`: The user model

The following variables are available in your controller:

- `self.user()`: Returns the current user
- `self.auth()`: Returns the webapp2 authentication provider


####6. Testing on localhost

In production the plugin sends unique *account activation* and *password reset* links via email.

In development the emails don't get sent, but the links get logged so you can use them.

Example:

1. Register at `/register`
2. Open GoogleAppEngineLauncher, click **Logs**.
3. Find one in the format: `confirmation_url: http://localhost:8080/activation/xxx/xxx`
4. Paste the addressin your browser, and your account will be activated

###How it works

Tutorials:

- [User authentication with webapp2 on Google App Engine](http://blog.abahgat.com/2013/01/07/user-authentication-with-webapp2-on-google-app-engine/)
- [User Accounts with Webapp2 + Google App Engine](http://gosurob.com/post/20024043690/gaewebapp2accounts)

Documentation:

- [Webapp2 Auth](https://webapp-improved.appspot.com/api/webapp2_extras/auth.html)

Also check out the source from GAE Boilerplate, where I copied most of this code from:

- [GAE Boilerplate](https://github.com/coto/gae-boilerplate)