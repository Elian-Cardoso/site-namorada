use App\Http\Controllers\EventoController;
use App\Http\Controllers\EventoFotoController;
use App\Http\Controllers\LojaController;
use App\Http\Controllers\ReviewController;
use App\Http\Controllers\GameController;
use App\Http\Controllers\CardController;

Route::get('/', function () {
    return view('welcome');
});

Route::get('/jogo', [GameController::class, 'jogo']);
Route::resource('cards', CardController::class);
Route::resource('eventos', EventoController::class);
Route::resource('evento-fotos', EventoFotoController::class);
Route::resource('lojas', LojaController::class);
Route::resource('reviews', ReviewController::class);
