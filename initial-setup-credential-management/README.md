Before you go any further you're going to want to have some kind of secure place to store all the secrets, accounts, passwords, API keys, etc that you'll be creating throughout this process.  

If you're storing them digitally, you need to store them somewhere that's encrypted.  There's a million solutions out there.  I'll be using [1Password](https://1password.com/) which does have a monthly subscription, so if that's not your thing you can get just as much mileage out of a free option like [Bitwarden](https://bitwarden.com/).

The browser/OS integrated options are file too, Chrome and Firefox have built in password managers and Apple has one along with a desktop app too.  Main thing is you want it to be multi-platform because ideally you'll want to access them on your Linux server as well.

If you want to store them physically then [maybe something like this](https://www.indigo.ca/en-ca/internet-password-logbook-cognac-leatherette-keep-track-of-usernames-passwords-web-addresses-in-one-easy-organized-location/9781631061943.html).  I'm mostly joking and not suggesting you actually do this, but it's a hell of a lot more secure than an unencrypted `.txt` file on your computer since it's impossible to hack by anyone except someone who is physically in your home.

Or you could go the self hosted route in line with all your other services and give [Vaultwarden](https://github.com/dani-garcia/vaultwarden) a try.

Anyway, regardless of which option you pick, all subsequent tutorials are going to presume that you have a secure and encrypted location to store sensitive data.  