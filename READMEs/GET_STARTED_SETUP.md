# Repo Setup
1) Navigate to the repo and click 'Fork'

![fork_button](./images/fork_button.png)

2) Navigate to your fork (it will probably redirect you [there](https://github.com/distraughtiana/nfl-pbp-viewer))


3) Copy the git@git.... link
* Make sure you choose the SSH option, otherwise you'll be prompted for user name and password w/ every push

![clone_button](./images/cloning_button.png)

4) Navigate to whatever folder you want to store the repo in and run:
* `$ git clone git@github.com:distraughtiana/nfl-pbp-viewer.git`

5) Run: `$ git branch` you should see: dev

6) Run: `$ git remote -v`

* You should see your fetch and push

![git_remote_before](./images/git_remote_before.png)

7) Run: `$ git remote add upstream git@github.com:DaCatBeam/nfl-pbp-viewer.git`

* This will allow you to pull code from the master repository to update your own fork.

8) Run: `$ git remote -v` again and you should see a new remote named 'upstream'

9) When creating a new branch you'll run:
* `$ git checkout -b <new_branch_name>`
* This will create a new branch and switch over to it.
* We'll start a convention of using the Jira Issue Key as our branch names. And prepend commit messages with it as well. This will make coding tasks more tracable and provide observability to project managers. It also has the added benefit of making breaking changes or defects easier to find and remove from the code base.
* Example: My branch is named: `NFLPBP-2`

# Post Repo Setup
* While on the dev branch, run: 
```bash
# Pull commit references down from the main repo, and into yours
$ git fetch upstream dev

# git pull from upstream dev branch into your local dev branch
$ git pull upstream dev

# Push the changes you added from upstream/dev to your remote repo
$ git push origin dev

# This should get you up to date, but check for the commit.
```

# ENV setup
1) After cloning, navigate to the repo. This should automatically make a "nfl-pbp-viewer" gemset if you've got ruby-2.7.1 installed.
* Check it with: `$ rvm gemset list`
* After you've created the gemset, you can make yourself a quick command in ~/.bashrc that'll save you time switching w/:
`alias nfl-pbp-viewer_ruby="rvm use ruby-2.7.1 && rvm gemset use nfl-pbp-viewer"`
* After this, check your email, you should have a short hexidecimal hash along w/ an ENV variable for you to add a little furthur down. For now, just copy the hexidecimal string, navigate to /nfl-pbp-viewer/config/ and create a file named "master.key" and paste that in there. Do not put a newline after it.

2) Install nodejs: `$ sudo apt install nodejs`

3) Install npm: `$ sudo apt install npm`

4) Install Yarn:
```bash
$ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
$ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
$ sudo apt update && sudo apt install yarn
```

5) Install Postgresql:
`$ sudo apt update && sudo apt install postgresql postgresql-contrib`

6) Add DB Server start/stop commands to .bashrc for easy aliases:
```bash
alias pg_start="sudo pg_ctlcluster 12 main start"
alias pg_stop="sudo pg_ctlcluster 12 main stop"
```

* Next, scrap the terminal and open a new one, then run `$ pg_start`

7) Run: `$ sudo su - postgres` to login to postgres DB server as the postgres super user

8) Create a postgres user for the app:
`$ createuser -P -d nfl_pbp_viewer`
* This will give us permission to create new DBs
* You'll be prompted for a password. It's in the email that I sent you w/ the RAILS master key. For now, add the following to your .bashrc file:

```bash
export NFL_PBP_VIEWER_DATABASE_PASSWORD="<The Password>"
```
* Note this is a really awful security practice that we'll change

* Then Run: `$ source ~/.bashrc` to load the ENV variable

9) Install the bundler gem in the new gemset: `$ gem install bundler`

10) Run: `$ bundle install`

11) Run `$ rails db:create`

12) In the new terminal, navigate to the repo root and run: `$ bin/webpack-dev-server`
* if this complains w/ a sudo permissions error, run: `$ chmod 755 bin/webpack-dev-server`

13) In another new terminal: navigate to the repo and run: `$ rails s` to start the rails server

14) Go to your `localhost:3000` in your browser. You should get the default "Welcome to Rails" page w/ some gem and architecture info displayed.
