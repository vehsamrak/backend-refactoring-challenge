[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/Vehsamrak/backend-refactoring-challenge/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/Vehsamrak/backend-refactoring-challenge/?branch=master)

## Background
This project provides some API endpoints to create a job. The actual endpoint to save the job is POST /jobs, 
while the other endpoints are providing some additional functionality that clients may need.

A job represents a task or some job a consumer needs to be done by a tradesman/craftsman.
It is something like "Paint my 60m2 flat" or "Fix the bathroom sink".
Every job is categorized in a "Service". You can think of them like categories. eg: "Boat building & boat repair" or "Cleaning of gutters".
Also every job needs some additional data like a description and when the job should be done.

## The Task
You should review and refactor this project, so it matches your criteria for good code and has a state that you're fine with. 
Feel free to change anything you want.
Please write a small documentation where you explain what you have changed and why you think your refactored code is better than the original code.

## Expectations
During our review we will go through your refactoring in order to check if expected changes have been made. The expected changes are defined according to industry code quality standards and on best practices. In order to help you a bit we have defined how much findings and refactoring actions we expect at least, following are the actions required prioritized by criticality.

### Critical
We expect at least 3 fixes that are critical issues

### Medium
Depending on your experience we will expect a certain number of findings here.
Hint, make sure you consider coding principles and design patterns in your refactoring.
There is a total of at least 4 to 7 findings here depending on your experience.

### Lower
This section is for less important changes but will give you bonus points, especially the more senior the role is the more we also expect to see here.
Hint, things like documentation, code cleanup, higher test coverage.


## Run the project
### Setup
- `docker-compose up -d`
- `docker run --rm --interactive --tty --volume $PWD/jobs:/app composer:2.0 install`
- `docker-compose exec php bin/console doctrine:migrations:migrate`

## Tests
- `docker-compose exec php bin/console doctrine:database:create --env=test`
- `docker-compose exec php vendor/bin/phpunit`

### TODO
* Use existing nginx container from registry instead of creating new one
* Inspect against PSRs
* Use symfony inspection
* Use scrutinizer inspection
* Use scrutinizer CI
* Test coverage 100%
* write a small documentation where you explain what you have changed and why you think your refactored code is better than the original code.
* Create an errorMessageHandler
* Use custom exceptions
* Swagger API documentation
* [Critical] Wrap exceptions. Uncaught PHP Exception (NotFoundHttpException)
* Fix phpdocs everywhere
* Run migrations on test database
* Export mysql configuration outside docker image

### In progress

### Done
* [Critical] Symfony cache directories removed from git.
* [Critical] Tests ServiceControllerTest::getOneServiceFound() and ServiceControllerTest::getOneServiceNotFound() are failed because undefined variable error in \AppBundle\Controller\ServiceController::getAction()
* [Critical] TargetEntity property in ORM\ManyToOne annotation in Job entity has typo. Fixed to valid related entity classname.
* [Critical] Fixed @ORM\JoinColumn annotation in Job entity related to zipcode points to wrong field (category_id).
* All TODOs removed from project.
* Return and argument types declared everywhere.
* Cache and logs moved to projects "var" directory to follow the principle of least surprise
* Test ZipcodeControllerTest::getOneZipcodeNotFound() failed due to wrong http code assertion
* Package "roave/securityadvisories" added to dependencies, to ensure application doesn't have installed dependencies with known security vulnerabilities.
* Move tests inside bundle
* Move fixtures to tests namespace
* Static services and builders removed from controllers. DI used instead. Controllers configured as services
* Builders renamed to factories and became non static
* Rename Service entity to JobCategory for better expression, and not to use reserved keyword. I would ask team and business side to rename it in Domain Dictionary as well, according to DDD principle. Rename MySQL table as well.
* Remove duplicated foreign keys (job_ibfk_3, job_ibfk_4). They are cascade, and can cause data loss in related tables
* Semantically rename foreign keys (job_ibfk_1, job_ibfk_2)
* Port was opened in MySQL container to enable external connection (make sence if docker used only for dev-environment)
* Type "String" renamed to "string" to follow PSR-12
* Commented code removed from Repositories
* Job entity validation moved to validator class. New custom validation constraint annotation implemented
* New unit tests naming strtegy applies pattern: "methodUnderTest_GivenState_ExpectedResult". Try-catch block used in tests instead of @expectedException annotation, to achieve AAA-pattern.
* JMSSerializer is used for DTO deserialization, while JsonSerializable interface is used for Entities serialization, because native serialization is much faster (no library overhead) and simpler to use for client code (less code, no dependencies, json_encode support). Explicit jsonSerialize on entities to define API contract. It is fragile to depend on property names.
* Builders removed. Requests are serialized into DTOs, validates and then goes to entity factories with explicit object mapping, instead of arrays usage.
* Remove app/Resources with unused views.
* IDE specific ".idea" was removed from job directory gitignore. Templates for various editors and operating systems must be presented in global gitignore config, not on the project level. Root level .gitignore was truncated to reduce duplication.
* Composer updated to 2.0. Cache volumes are stored correctly
* Persist database volumes, so they shall not be deleted when containers stops
* Autogenerated app/config/parameters.yml removed from git and gitignored
* Job entity has annotation @ORM\GeneratedValue with UUID strategy, and therefore should not have id in constructor
* Generate job entity uuid on backend instead of client or database. Custom IdGenerator generator created, because Doctrine generator uses database SELECT to create UUID. It is better for testing and performance to have generator on the application side.
* SQL should be present only in repositories. Job service refactored to create SQL with query builder.
* Validate DTOs
* Get rid of static methods
* Remove apache configuration files
* Strict types declaration in every php file
* Deprecated XDebug parameters changed to new values in PHP Dockerfile
* Configuration files cleaned up. Unused packages removed
* Fixed wrong validation maxMessage in Zipcode "city".
* Immutable datetime used in getters to prevent changing datetime object by reference.
* Review all tests. Tests with AAA
* Property names in entities formatted to camelCase, to follow PSR-1 (consistency with properties in other classes).
* Move services configuration inside bundle

### Could be done in future
* Implementation of all CRUD methods for Job, JobCategory and Zipcode.
* Zipcodes are standartized and static, so no need to store them in database. Could be extracted to configuration.
* Authentification and authorization for controller actions must be done.
* Symfony can be updated to latest version. Current project 3.4 is not maintained anymore.
* Logging should be applied.
* SQLite could be used for functional tests instead of MySQL.
* PHP version locked in composer.json could be raised.
* "Entity services" layer had transparent logic, so I moved logic to controllers. Because I dont like to see HTTP and validation logic outside controllers, and services before refactoring does not perform other actions. If there would be some buisiness logic requirements in future, I would create Factories to create entities. So for now I chose simplicity over preoptimization.
* PHP sessions could be stored in Redis, instead of file system for better performance due to slow disk read/write.
