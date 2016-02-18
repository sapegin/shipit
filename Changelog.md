# 0.1.0 - 2016-02-18

## Local targets

Now you can define targets that will be executed on your local machine. For example you can deploy with `rsync`:

```
host='myhost'
path='sites/example.com'

[deploy:local]
npm test
npm run build
rsync --archive --compress --force --delete public/ $SSH_HOST:$SSH_PATH
```

Local target will be executed before remote target with the same name.

# 0.0.6 - 2015-10-13

* Fix host name in copy command (by (@aerth)[https://github.com/aerth]).

# 0.0.5 - 2015-07-14

* New command: copy.

# 0.0.4 - 2015-02-03

* Exit when directory doesn't exist (by [@albburtsev](https://github.com/albburtsev)).
* Stop deploy on first error.
* Show error message on failed deploy.

# 0.0.3 - 2015-01-29

* Enable SSH keys forwarding (by [@albburtsev](https://github.com/albburtsev)).

# 0.0.2 - 2013-12-03

* Fix default target invoking.

# 0.0.1 - 2013-11-14

* First release.
