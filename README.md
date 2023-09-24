# AngularSslDevserver

This repository shows you an example of how to run your local angular dev server with SSL enabled.

A few steps needs to be done before it works.

1 - You need to add a local entry in your host file for the domain you want to use. For that purpose, you need to edit your host file and point the FQDN you want to use to `127.0.0.1`
> While you could generate a certificate for `localhost` locally and add it to the trust certificates, I feel like having a valid FQDN is closer to the reality of a prod deployed application.

2 - Get a certificate for the given FQDN you choosed to use as your local dev URL. A few providers allows you to create certificates for non-public URL. I personnaly like to use [ZeroSSL](https://zerossl.com/) which can generate 90 days certicates for non-public domain by validating them by email. If you own a domain, you could create a sub-domain pointing to `127.0.0.1` and get a certificate for it via `Acme`.

3 - Once you have your certicate, simply drop it into the `certs` folder. Please note that this folder must be included in the `.gitignore` so your certificates doesn't end up in your repository for obvious security reasons.

4. You need to add a few lines into the `angular.json` file to tell it to enable SSL and use your certicate. Underneat the `serve:` section, add the following :
```
"options": {
            ... (any previously precent option)
            "sslKey": "./certs/privkey.pem",
            "sslCert": "./certs/cert.pem",
            "ssl": true,
            "host": "<your domain>"
          },
```
5. Now that these configs are in place, you're ready! Fire it up with the classic `npm run start` and see the magic happening. You'll have access to your local angular app through the host you specified on the previous steps, through SSL.

## The warning

Angular will show you the following warning :
>Warning: This is a simple server for use in testing or debugging Angular applications
locally. It hasn't been reviewed for security issues.

>Binding this server to an open connection can result in compromising your application or
computer. Using a different host than the one passed to the "--host" flag might result in
websocket connection issues. You might need to use "--disable-host-check" if that's the
case.

This is fine in developement. That being said, **you should never run your angular app in production or any other environment then local development using the `npm run start` command**.
