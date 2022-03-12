# Install a Pimcore v5.8.9

## Docker-Compose consists of the following images:
<pre>
- Redis
- MariaDB 10.4
- Apache
</pre>


## init and startup containers
<pre>
docker stack deploy -c docker-compose-pimcore.yml <_STACKNAME_>
</pre>


## exec in pimcore-container
<pre>
docker exec -it <_PIMCORE CONTAINER_> bash
</pre>


## install create composer-project for pimcore (specific package)
> lookup https://packagist.org/packages/dpfaffenbauer/pimcore-skeleton


## update composer for enhancing performance (if necessary) 
<pre>
composer self-update
</pre>


## create new project
<pre>
COMPOSER_MEMORY_LIMIT=-1 composer create-project dpfaffenbauer/pimcore-skeleton:5.4.1 tmp
</pre>


## fix folder structure
<pre>
mv tmp/.[!.]* . && mv tmp/* . && rmdir tmp
</pre>


## add to pimcore > src > AppBundle > Resources > config > service.yml ... to make monolog working
<pre>
# fix monolog-bundle
Pimcore\Log\Handler\ApplicationLoggerDb:
    public: true
    autowire: false
    autoconfigure: false
    calls:
        - [pushProcessor, ['@monolog.processor.psr_log_message']]
        - [pushProcessor, ['@Pimcore\Log\Processor\ApplicationLoggerProcessor']]
        - [pushProcessor, ['@pimcore.app_logger.introspection_processor']]
</pre>


## install pimcore
<pre>
./vendor/bin/pimcore-install --mysql-host-socket=db --mysql-username=pimcore --mysql-password=pimcore --mysql-database=pimcore
</pre>


## After the installer is finished, you can open in your Browser:
<pre>
Frontend: http://localhost:5000
Backend: http://localhost:5000/admin
Adminer: http://localhost:5002
</pre>


## Common Errors

 - File permissions

<pre>
docker exec -it <_PIMCORE CONTAINER_> bash 
chown www-data: . -R 
</pre>
