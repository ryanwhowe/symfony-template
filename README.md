# symfony-template
This is a template for a docker based development of a symfony based application with nginx.

___Uses:___ 
* php 8.5
* nginx 1.30 (stable)

## Templates
There are two issue templates for GitHub included in the repo.  These can prompt the reporters to generate good issues.
In addition, there is a Pull Request Template to help keep the communication focus on the PR to help create quality pull
requests.  These templates should be updated and expanded upon to meet your project's needs.

### Bug Report
Bug reports that stress reproduction steps, expected behavior and screenshots

### Feature Request
Feature requests that keep the focus on the problem that needs to be resolved over how the reported feels it should be
solved with prompts for additional information

## Setup docker environment
Assuming that you have already installed docker on your local machine.  You will need the full path to the solution
folder.  Once the solution is cloned you will need to copy the `devops/dev/compose.override.yaml.example` to
`devops/dev/compose.override.yaml`.  Every place that that has `<path to solution>` will need to be replaced with
the fully qualified path to the base folder of the solution, should be a folder path ending in the name of your repository unless you cloned
the repo to a custom folder name.  The project `.gitignore` will not allow this file to be committed to the repo as it is
dependent on the local environment

From the project root
```shell
cp devops/dev/compose.override.yaml.example devops/dev/compose.override.yaml
```

### Run the docker solution
The first time that this solution is run it will need to build the project containers and download the base containers
used in the project.  This initial build can take some time, grab a coffee after executing the up command for the first
time.

From the project root directory
```shell
cd devops/dev
docker compose up -d
```

### command line access to php container
From the `devops/dev` directory
```shell
docker compose exec php bash
```

### Xdebug
Xdebug connects to the host IDE on port 9003 through `host.docker.internal`. Debugging starts
only when an Xdebug trigger is present, such as the `XDEBUG_TRIGGER` environment variable or the browser extension for
your IDE. For example, to debug a console command from the `devops/dev` directory:

```shell
docker compose exec -e XDEBUG_TRIGGER=1 php php bin/console about
```

## Installing Symfony
The `symfony` directory is empty. The included `php.Dockerfile` is built from the official PHP 8.5 FPM image. The
resulting image contains both Composer and the Symfony CLI already installed.

Install the application from inside the running PHP container so that it uses the PHP version and tools provided by the
container. From the `devops/dev` directory, open a shell in the container:

```shell
docker compose exec php bash
```

Then, from inside the container, create a full Symfony web application:

```shell
cd /var/www/symfony
symfony new . --webapp --no-git
```

The `--webapp` option installs the standard web application dependencies. The `--no-git` option prevents Symfony from
creating a nested Git repository inside the existing project repository. For an API-only application, use
`symfony new . --api --no-git` instead. To select a specific Symfony version, add a version such as
`--version="lts"` or `--version="stable"` to the command.

After installation, verify the application from inside the container:

```shell
php bin/console about
exit
```

You can install additional packages from the same container with `composer require`. For example:

```shell
docker compose exec php composer require symfony/orm-pack
```

For additional project types, Symfony version selectors, the demo application, Composer-based installation, and
technical requirements, see the official Symfony [documentation for creating Symfony applications](https://symfony.com/doc/current/setup.html#creating-symfony-applications).

## Accessing the solution via http
The included `nginx.Dockerfile` will make the symfony solution available from your local environment on port 8081.
Navigate you web browser to http://localhost:8081 to view the solution.

If you are creating a cli application that does not utilize a web interface you can remove the `images/nginx` directory, 
`nginx.Dockerfile` and `/logs` directory from the solution along with the accompanying settings in the 
`compose.yaml`, `compose.override.yaml.example` and your `compose.override.yaml` files.

## logs
The nginx server will log to the solutions `/logs/nginx` directory

## Template Usage
Please update this README file to suit the needs of your project, I only ask that you please reference the template
project if you base a solution off it.  If possible please keep below this line when you use this template for your project

---
<dl>
    <dt>
        <em>Based of the <a href="https://github.com/ryanwhowe/symfony-template">symfony-template</a> GitHub Template project</em>
    </dt>
    <dd>
        <strong>by <a href="https://github.com/ryanwhowe" target="_blank">Ryan Howe</a></strong>
    </dd>
</dl>
