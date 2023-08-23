# Installing psql on MacOS

Installing `psql` without the full Postgres install

[Link](https://stackoverflow.com/questions/44654216/correct-way-to-install-psql-without-full-postgres-on-macos)

You could also use homebrew to install libpq.

`brew install libpq`

This would give you psql, pg_dump and a whole bunch of other client utilities
without installing Postgres.

Unfortunately since it provides some of the same utilities as are included in
the full postgresql package, brew installs it "keg-only" which means it isn't in
the PATH by default. Homebrew will spit out some information on how to add it to
your PATH after installation. In my case it was this:

`echo 'export PATH="/usr/local/opt/libpq/bin:$PATH"' >> ~/.zshrc`
