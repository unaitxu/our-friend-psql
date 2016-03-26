# our-friend-psql
* To create a user for Rails DB

```
CREATE ROLE postgres WITH PASSWORD 'password' CREATEDB LOGIN;
```

* To check if postmaster.pid already exists (and to restart postgres after killing it)

```
postgres -D /usr/local/var/postgres
```

* While not really an psql trick, useful to stay on a 'stable for you' version of the db
```bash
brew info postgresql
brew switch postgresql {version}
brew pin postgresql
# To update again
brew unpin postgresq
```

* To update from 9.4.5_2 to 9.5 (it's always a good idea to have an updated backup :wink: )
```bash
# Stop postgres
launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Update homebrew
brew update
brew upgrade --all

# With psql updated to 9.5
initdb /usr/local/var/postgres9.5 -E utf8

# Run the pg_upgrade cmd
pg_upgrade -v
  -d /usr/local/var/postgres \                    # The old db directory
  -D /usr/local/var/postgres9.5 \                 # The new db
  -b /usr/local/Cellar/postgresql/9.4.5_2/bin/ \  # Where the old psql version is
  -B /usr/local/Cellar/postgresql/9.5.0/bin/      # Where the new psql version is

# In /usr/local/var, move old data and replace it with the new
mv postgres postgres9.4
mv postgres9.5 postgres

# Restart psql
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Probably it'll be a nice idea to update the pg gem
gem uninstall pg
gem install pg
```
