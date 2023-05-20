# Laravel 9 REST API with Sanctum Auth

Create New Project

```
composer create-project --prefer-dist laravel/laravel laravel-sanctum-auth
```

Add database details in .env configuration file

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=db_name
DB_USERNAME=root
DB_PASSWORD=
```

Add sanctum package
```
composer require laravel/sanctum
```

Register the sanctum middleware into the api array inside the app/Http/Kernel.php file

```
protected $middlewareGroups = [
...
...
    'api' => [
        \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
        'throttle:api',
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],
...
...
];
```
Execute command for generating database tables

```
php artisan migrate
```
Create BaseController file for managing response

```
<?php

namespace App\Http\Controllers\API;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller as Controller;

class BaseController extends Controller
{
    /**
     * success response method.
     *
     * @return \Illuminate\Http\Response
     */
    public function sendResponse($result, $message)
    {
    	$response = [
            'success' => true,
            'data'    => $result,
            'message' => $message,
        ];
        return response()->json($response, 200);
    }

    /**
     * return error response.
     *
     * @return \Illuminate\Http\Response
     */
    public function sendError($error, $errorMessages = [], $code = 404)
    {
    	$response = [
            'success' => false,
            'message' => $error,
        ];
        if(!empty($errorMessages)){
            $response['data'] = $errorMessages;
        }
        return response()->json($response, $code);
    }
}
```

Now creates AuthConroller file and add below code
```
<?php
   
namespace App\Http\Controllers\API;
   
use Illuminate\Http\Request;
use App\Http\Controllers\API\BaseController as BaseController;
use Illuminate\Support\Facades\Auth;
use Validator;
use App\Models\User;
   
class AuthController extends BaseController
{
    public function login(Request $request)
    {
        if(Auth::attempt(['email' => $request->email, 'password' => $request->password])){ 
            $authUser = Auth::user(); 
            $success['token'] =  $authUser->createToken('MyAuthApp')->plainTextToken; 
            $success['name'] =  $authUser->name;
   
            return $this->sendResponse($success, 'User signed in');
        } 
        else{ 
            return $this->sendError('Unauthorised.', ['error'=>'Unauthorised']);
        } 
    }
    public function logout(Request $request)
    {
        $validator = Validator::make($request->all(), [
            'name' => 'required',
            'email' => 'required|email',
            'password' => 'required',
            'confirm_password' => 'required|same:password',
        ]);
   
        if($validator->fails()){
            return $this->sendError('Error validation', $validator->errors());       
        }
   
        $input = $request->all();
        $input['password'] = bcrypt($input['password']);
        $user = User::create($input);
        $success['token'] =  $user->createToken('MyAuthApp')->plainTextToken;
        $success['name'] =  $user->name;
   
        return $this->sendResponse($success, 'User created successfully.');
    }

    public function logout(Request $request){
        Auth::user()->tokens()->delete();
        $success = true;
        return $this->sendResponse($success, 'User logout successfully.');
    }
   
}
```
routes/api.php

```
Route::post('login', [AuthController::class, 'login']);
Route::post('register', [AuthController::class, 'register']);

Route::middleware('auth:sanctum')->group( function () {

    // Logout
    Route::get('logout', [AuthController::class, 'logout']);

    // Blog Resources
    Route::resource('blogs', BlogController::class);
});

```
Now, Article crud opertaions
```
<?php

namespace App\Http\Controllers\API;

use App\Http\Controllers\API\BaseController;
use App\Http\Resources\ArticleResource;
use App\Models\Article;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Validator;
use Illuminate\Support\Str;

class ArticleController extends BaseController
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        // $articles = Article::with('user')->paginate(10);
        $articles = ArticleResource::collection(Article::with('user')->paginate(2));
        return $this->sendResponse($articles, '');
    }

    /**
     * Show the form for creating a new resource.
     */
    public function create()
    {
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
        $user_id = Auth::user()->id;
        $uuid = Str::uuid();

        $validator = Validator::make($request->all(), [
            'title' => 'required',
            'description' => 'required'
        ]);
        if($validator->fails()){
            return $this->sendError($validator->errors());
        }

        $item = new Article();
        $item->title = $request->title;
        $item->uuid = $uuid;
        $item->user_id = $user_id;
        $item->description = $request->description;
        $item->save();

        return $this->sendResponse($item, 'Data created successfully.');
    }

    /**
     * Display the specified resource.
     */
    public function show(string $id)
    {
        $article = Article::find($id);
        if (is_null($article)) {
            return $this->sendError('Article did not found.');
        }
        return $this->sendResponse($article, '');
    }

    /**
     * Show the form for editing the specified resource.
     */
    public function edit(string $id)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, string $id)
    {
        $item = Article::find($id);

        $validator = Validator::make($request->all(), [
            'title' => 'required',
            'description' => 'required'
        ]);
        if($validator->fails()){
            return $this->sendError($validator->errors());
        }

        $item->title = $request->title;
        $item->description = $request->description;
        $item->save();

        return $this->sendResponse($item, 'Data updated successfully.');
    }

    /**
     * Remove the specified resource from storage.
     */
    public function destroy(string $id)
    {
        $item = Article::destroy($id);
        return $this->sendResponse([], 'Data deleted successfully.');
    }
}
```
Article Resources
```
<?php

namespace App\Http\Resources;

use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;
use Illuminate\Http\Resources\Json\ResourceCollection;

class ArticleResource extends JsonResource
{
    /**
     * Transform the resource collection into an array.
     *
     * @return array<int|string, mixed>
     */
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'uuid' => $this->uuid,
            'title' => $this->title,
            'image' => $this->image,
            'description' => $this->description,
            'user' => new UserResource($this->whenLoaded('user')),
            'created_at' => $this->created_at,
            'updated_at' => $this->updated_at,
        ];
    }
}

```
Article Model
```
class Article extends Model
{
    use HasFactory;

    protected $fillable = [
        'id',
        'title',
        'description',
    ];

    public function users(): BelongsTo
    {
        return $this->belongsTo(User::class)->as("users");
    }



    public function user(): HasOne
    {
        return $this->hasOne(User::class, 'id');
    }
}
```
User Model
```
class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array<int, string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * The attributes that should be cast.
     *
     * @var array<string, string>
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
        'password' => 'hashed',
    ];

    public function articles(): HasMany
    {
        return $this->hasMany(Article::class, 'user_id');
    }
}
```
User Resources
```
class UserResource extends JsonResource
{
    /**
     * Transform the resource collection into an array.
     *
     * @return array<int|string, mixed>
     */
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email
        ];
    }
}
```
User Controller
```
class UserController extends BaseController
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        $users = User::with('articles')->paginate(2);
        // $users = UserResource::collection(User::with('articles')->paginate(2));
        return $this->sendResponse($users, '');
    }
}
```

