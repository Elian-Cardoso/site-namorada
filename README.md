namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class EventoFoto extends Model
{
    protected $fillable = ['evento_id', 'caminho_foto'];

    public function evento()
    {
        return $this->belongsTo(Evento::class);
    }
}
