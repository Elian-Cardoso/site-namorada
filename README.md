namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Evento extends Model
{
    protected $fillable = ['nome', 'data_evento'];

    public function fotos()
    {
        return $this->hasMany(EventoFoto::class);
    }
}
