---
title: Laravel
copyright: true
date: 2016-10-02 20:50:32
tags:
  - PHP
  - Laravel
---


## 初始化项目

```bash
# 使用 Laravel 命令行工具
# 确保已将 `~/.composer/vendor/bin` 加入 PATH
composer global require "laravel/installer"
laravel new blog

# 或者直接使用 Composer
# 考虑缓存，dist 包优先
# --perfer-dist: 强制使用压缩包，而不是克隆源代码
composer create-project --prefer-dist laravel/laravel blog
```

## 启用服务
```bash
# 使用 PHP 内置服务器，并指定端口
php -S localhost:8888 -t public

# 或者使用 artisan，默认端口 8000
php artisan serve

# ============== 分割线 ================
# 启用维护模式
# 维护模式的响应模板放在 resources/views/errors/503.blade.php
# 维护模式有几秒种的服务器不可用时间，想平滑迁移可以使用 Envoyer 服务
php artisan down
# message 为显示给用户的信息，retry 作为 Retry-After HTTP 标头返回
php artisan down --message='Upgrading Database' --retry=60

# 关闭维护模式
php artisan up
```

## artisan
```bash
# 新建控制器
php artisan make:controller SitesController
# 创建没有预定义方法的控制器
php artisan make:controller TestController --plain

# migrate
php artisan migrate
# migrate 回滚
php artisan migrate:rollback
# 新建 migrate 文件
# --create 指明表名
php artisan make:migration create_article_table --create=articles

# 建立添加列的 migration 文件
php artisan make:migration add_intro_column_to_articles --table=articles
php artisan migrate


# 建立 model
php artisan make:model Article

# 进入 tinker
php artisan tinker

# 部署的时候运行，以缓存配置信息来提升应用程序速度
# 所有配置文件将被缓存到单一文件
php artisan config:cache
```

## tinker
```tinker
$article=new App\Article;
$article->title='My first Title';
$article->content='content';
$article->published_at=Carbon\Carbon::now();
$article->save();
$article->toArray();

# 查询
$first=App\Article::find(1);
$first->title='Update';
$first->save();

$second=App\Article::where('content', '=', 'content')->get();
// 默认为相等关系
$second=App\Article::where('content', 'content')->get();
// 拿第一条数据
$second=App\Article::where('content', 'content')->first();

// 填充数据
$article=App\Article::create([title'=>'Second Title', 'content'=>'Second Content', 'published_at'=>Carbon\Carbon::now()]);

// 更新数据
$article->update(['title'=>'Change Title']);
```


## 项目结构

### 关键文件
- `app/Http/routes.php` 路由
- `resources/views/` 存放视图文件，如 `resources/views/welcome.blade.php`
- `app/Http/Controllers/` 存放控制器
- `.env` 环境变量配置文件
- `config/` 存放各个切面的具体配置文件
- `database/migrations/` 存放 migrate 文件
- `app/` model 默认将放置于此
- `app/Http/Requests/` 放置 `Request`


## 配置文件

### app.php
```php7
return array(
  /**
    默认为 'UTC' 时区，国内网站建议设为 'PRC' 或 'Asia/Shanghai' 时区
   */
  'timezone' => 'Asia/Shanghai'
);
```
环境变量集中放到 `.env` 下配置
在配置文件中使用 `env('DB_HOST', 'localhost')` 取用 `.env` 下的变量
```php
// 读取环境变量，第二个参数为默认值
$value = env('APP_DEBUG', false);

// 取出配置信息
$value = config('app.timezone');

// 运行时配置
config(['app.timezone' => 'America/Chicago']);

// 读取和判断应用程序当前环境（`.env` 里的 `APP_ENV`）
$environment = App::environment();
if (App::environment('local')) {
  // 当前正处于开发环境
}
if (App::environment('local', 'staging')) {
  // 当前环境处于 local 或者 staging
}
```


## 路由
### 终端命令
```bash
# 查看所有路由
php artisan route:list
```
- 返回 `resources/views/sites/about.blade.php` 页面可以 `return view('sites/about');`，也可以 `return view('sites.about');`
- 使用控制器 `Route::get('/', 'SitesController@index');`
- `Route::get('/article/{id}/edit', 'ArticlesController@edit');`

### 终极
```php
// 自动注册常规路由
Route::resource('articles', 'ArticlesController');
```

### 可变路由
```php
Route::get('/articles/{id}', 'ArticlesController@show');
```

## 控制器
- 传递参数到视图 `return view('sites.about')->with('name', $name);`
- 传递多个参数到视图
```php
return view('sites.about')->with([
  'first' => 'Jelly',
  'last' => 'Bool'
]);
// 或者 $data 为数组时
return view('sites.about', $data);
// 有或者使用 compact
$first = 'Jelly';
$last = 'Bool';
return view('sites.about', compact('first', 'last'));
$people = ['Taylor Otwell', 'Jeffray Way', 'Happy Peter'];
return view('sites.about', compact('people'));
return view('articles.show', compact($article));
```

### 示例
```php
// 取得传入的值
$request->get('title');
public function store(Request $request)
{
  $input = $request->all();
  $input['published_at'] = Carbon::now();
  // Article::create 会自动过滤掉 _token
  Article::create($input);
  return redirect('/articles');
}

public function index()
{
  $articles = Article::latest()->get();
  return view('articles.index', compact('articles'));
}

public function edit()
{
  $article = Article::findOrFail($id);
  return view('articles.edit', compact('article'));
}

public function update(Request $request, $id)
{
  $article = Article::findOrFail($id);
  $article->update($request->all());

  return redirect('/articles');
}
```

### 结合表单验证
```php
public function store(Requests\CreateArticleRequest $request)
{
  Article::create($request->all());
  return redirect('/articles');
}
// 或
public function store(Request $request)
{
  $this->validate($request, ['title'=>'required', 'content'=>'required']);
  Article::create($request->all());
  return redirect('/articles');
}
```

## Carbon 类
```php
$article->create_at->diffForHumans();
```


## Blade模板引擎

### 使用参数
- 使用 `{{ $value }}`
- 不需要转义 `{!! $value !!}`
- `{% raw %}href="{{ url('articles', $article->id) }}"{% endraw %}`
- `{% raw %}href="{{ action('ArticlesController@show', [$article->id]) }}"{% endraw %}`


### 安装依赖
```bash
composer require illuminate/html
```
将 `Illuminate\Html\HtmlServiceProvider::class` 添加到 `app.providers` 里
将 `Illuminate\Html\FormFacade::class` 添加到 `app.aliases` 里

### 使用 Laravel Forms
```html
<!--
  编辑页面中
  PATCH 为 Laravel 独有
  {!! Form::open(['method'=>'PATCH', 'url'=>'/articles'.$article->id]) !!}
-->
{!! Form::open(['url'=>'/articles']) !!}
  <div class="form-group">
  {!! Form::label('name') !!}
  {!! Form::text('name', null, ['class'=>'form-control']) !!}
  </div>
  <div class="form-group">
  {!! Form::label('published_at', 'Published_at:') !!}
  {!! Form::input('data', 'published_at', date('Y-m-d'), ['class'=>'form-control']) !!}
  </div>
  {!! Form::submit('发表文章', ['class'=>'btn btn-primary form-control']) !!}
{!! Form::close() !!}
@if($errors->any())
<ul class="list-group">
@foreach($errors->all() as $error)
  <li class="list-group-item list-group-item-danger">{{ $error }}</li>
@endforeach
</ul>
@endif
```

```php
{!! Form::model($article, ['method'=>'PATCH', 'url'=>'/articles'.$article->id]) !!}
  <div class="form-group">
  {!! Form::label('name') !!}
  {!! Form::text('name', null, ['class'=>'form-control']) !!}
  </div>
  <div class="form-group">
  {!! Form::label('published_at', 'Published_at:') !!}
  {!! Form::input('data', 'published_at', date('Y-m-d'), ['class'=>'form-control']) !!}
  </div>
  {!! Form::submit('发表文章', ['class'=>'btn btn-primary form-control']) !!}
{!! Form::close() !!}
@include('errors.list')
```
5.2 路由要写这里边，要不然会有未定义 `$errors` 报错
```php
Route::group(['middleware' => ['web']], function () {
    Route::get('/articles','ArticlesController@index');
    Route::get('/articles/create','ArticlesController@create');
    Route::get('/articles/{id}','ArticlesController@show');
    Route::post('/articles','ArticlesController@store');
});
```

farea + Tab 补全
ftext + Tab

### 使用布局页

#### 布局页
```php
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  @yield('content')
  @yield('footer')
</body>
</html>
```

#### 内容页
```php
@extends('app') // app 为布局页文件名
@section('content')
内容放置于此，一个 @section 对应一个 @stop
@stop
@section('footer')
内容
@stop
```

### 条件判断
```php
@if($first == 'Taylor Otwell')
内容
@else
内容
@endif
```

### 循环
```php
@if(count($people) > 0)
<ul>
@foreach($people as $person)
<li>{{ $person }}</li>
@endforeach
</ul>
@endif
```

## migrate

### 终端命令
```bash
# 数据库数据将被抹除
php artisan migrate:refresh

php artisan make:migration add_user_id_column_to_articles
```

```tinker
$user=App\User::first();
$user->articles
# 或者加上条件，链式操作需要括号
$user->articles()->where()

$article=App\Article::first();
$article->user->name;
```

### 建表示例
```php
public function up()
{
  Schema::create('articles', function (Blueprint $table) {
    $table->increments('id');
    $table->string('title');
    $table->text('content')->default('test');
    $table->timestamp('published_at');
    $table->timestamps();
  })
}
```

### 添加列示例
```php
public function up()
{
  Schema::table('articles', function (Blueprint $table) {
    $table->string('intro');
  })
}

public function down()
{
  Schema::table('articles', function (Blueprint $table) {
    $table->dropColumn('intro'); // 可能需要额外引入 doctrine/dbal 包
  })
}
```

## model

### 常用方法
```php
$article = Article::findOrFail($id)
```
待处理
```php
Article::where('id', $id)->update($input)
Article::where('id','=',$id)->update($request->except('id','_method','_token','/article/'.$id));
```

### 设置字段可填充
```php
class Article extends Model
{
  protected $fillable = ['title', 'content', 'published_at'];
}
```

```php
class Article extends Model
{
  protected $fillable = ['title', 'content', 'published_at'];

  protected $datas = ['published_at']; // 自动转为 Carbon 对象

  public function setPublishedAtAttribute($date)
  {
    $this->attributes['published_at'] = Carbon::createFromFormat('Y-m-d', $data);
  }

  public function scopePublished($query)
  {
    $query->where('published_at', '<=', Carbon::now());
  }
}
```

### 取出表中所有数据
```php
$article = Article::all();

// 以更新顺序排序
Article::latest('updated_at')
```

## 辅助函数
```php
// die and down
dd($article)
```

## 表单验证

### 两种表单验证的方式
1. Request
2. validate

### 终端命令
```bash
php artisan make:request CreateArticleRequest
```

### Request示例
```php
class CreateArticleRequest extends Request
{
  public function authorize()
  {
    return true;
  }

  public function rules()
  {
    return [
      'title' => 'required|min:3',
      'content' => 'required',
      'published_at' => 'required'
    ];
  }

  // 不同规则
  public function rules()
  {
    $rules = [
      'title' => 'required|min:3',
      'content' => 'required',
      'published_at' => 'required'
    ];
    if(isEdit()) {
      $rules['something'] = 'required';
    }
    return $rules
  }
}

```

## 优化
表单错误显示
`resources/views/errors/list.blade.php`
```php
@if($errors->any())
<ul class="list-group">
@foreach($errors->all() as $error)
  <li class="list-group-item list-group-item-danger">{{ $error }}</li>
@endforeach
</ul>
@endif
```