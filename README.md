Assuming you have this directory structure:

    ./citypantry-3-api
    ./citypantry-3-frontend
    ./citypantry-2-puppet
    ./Vagrantfile

First install our dependencies with `git submodule update --init --resursive`.

Run `vagrant up`. This will provision you a Vagrant box which you can SSH to via `vagrant ssh`.

Add to your `/etc/hosts`:

    192.168.33.10  api.citypantry.dev order.citypantry.dev uploads.citypantry.dev

When your Vagrant box is running, the site will be accessible at `http://order.citypantry.dev/`

To sync your API and frontend folders, run `nohup vagrant rsync-auto &`

Useful commands to alias:

* Reload database fixtures: `vagrant ssh -c '/home/citypantry/project/api/app/console doctrine:mongodb:fixtures:load'`
