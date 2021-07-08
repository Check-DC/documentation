# Laravel Best Practices
Laravel is a PHP framework and just like any other PHP framework there are several ways to get a single thing done in a lot of cases, for conformity these are hand picked best practices.

1. Installation of every Laravel project should be done with:
```bash
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
10. A class and a method should have only one responsibility.
11. Put all DB related logic into Eloquent models or into Repository classes if you're using Query Builder or raw SQL queries.
```php
public function index()
{
    return view('index', ['clients' => $this->client->getWithNewOrders()]);
}

class Client extends Model
{
    public function getWithNewOrders()
    {
        return $this->verified()
            ->with(['orders' => function ($q) {
                $q->where('created_at', '>', Carbon::today()->subWeek());
            }])
            ->get();
    }
}
```
12. Reuse code when you can. SRP is helping you to avoid duplication. Also, reuse Blade templates, use Eloquent scopes etc.
```php
public function scopeActive($q)
{
    return $q->where('verified', 1)->whereNotNull('deleted_at');
}

public function getActive()
{
    return $this->active()->get();
}

public function getArticles()
{
    return $this->whereHas('user', function ($q) {
            $q->active();
        })->get();
}
```
13. Eloquent allows you to write readable and maintainable code. Also, Eloquent has great built-in tools like soft deletes, events, scopes etc.
```php
Article::has('user.profile')->verified()->latest()->get();
```
14. Document your code, but prefer descriptive method and variable names over comments
```
if ($this->hasJoins())
```
15. Follow naming conventions accepted by Laravel community and PSR2

What | How | Good | Bad
------------ | ------------- | ------------- | -------------
Controller | singular | ArticleController | ~~ArticlesController~~
Route | plural | articles/1 | ~~article/1~~
Named route | snake_case with dot notation | users.show_active | ~~users.show-active, show-active-users~~
Model | singular | User | ~~Users~~
hasOne or belongsTo relationship | singular | articleComment | ~~articleComments, article_comment~~
All other relationships | plural | articleComments | ~~articleComment, article_comments~~
Table | plural | article_comments | ~~article_comment, articleComments~~
Pivot table | singular model names in alphabetical order | article_user | ~~user_article, articles_users~~
Table column | snake_case without model name | meta_title | ~~MetaTitle; article_meta_title~~
Model property | snake_case | $model->created_at | ~~$model->createdAt~~
Foreign key | singular model name with _id suffix | article_id | ~~ArticleId, id_article, articles_id~~
Primary key | - | id | ~~custom_id~~
Migration | - | 2017_01_01_000000_create_articles_table | ~~2017_01_01_000000_articles~~
Method | camelCase | getAll | ~~get_all~~
Method in resource controller | [table](https://laravel.com/docs/master/controllers#resource-controllers) | store | ~~saveArticle~~
Method in test class | camelCase | testGuestCannotSeeArticle | ~~test_guest_cannot_see_article~~
Variable | camelCase | $articlesWithAuthor | ~~$articles_with_author~~
Collection | descriptive, plural | $activeUsers = User::active()->get() | ~~$active, $data~~
Object | descriptive, singular | $activeUser = User::active()->first() | ~~$users, $obj~~
Config and language files index | snake_case | articles_enabled | ~~ArticlesEnabled; articles-enabled~~
View | kebab-case | show-filtered.blade.php | ~~showFiltered.blade.php, show_filtered.blade.php~~
Config | snake_case | google_calendar.php | ~~googleCalendar.php, google-calendar.php~~
Contract (interface) | adjective or noun | AuthenticationInterface | ~~Authenticatable, IAuthentication~~
Trait | adjective | Notifiable | ~~NotificationTrait~~





