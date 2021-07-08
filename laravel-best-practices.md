# Laravel Best Practices
Laravel is a PHP framework and just like any other PHP framework there are several ways to get a single thing done in a lot of cases, for conformity these are hand picked best practices.

1. Installation of every Laravel project should be done with:
```
composer create-project laravel/laravel {./} or {projectname-backend}
```
2. The project name should be in kebab cases in the format: `{projectname-backend}`
3. All file names should be in title cases **ProductController, PostService, Category**
4. All services should be named after model name if tied to a model, else it should carry the name of the entity, and the suffix 'Service' i.e. {Name}Service
5. The RouterServiceProvider should be used for route patterns.
6. The code editor or IDE should always have the PHPCS setup and active on it.
7. PSR2 standard should be used in the PHPCS setup.
8. Every function should have only one responsibility
9. When several models uses a certain logic or function that performs similar operation, a Trait should be created. 
