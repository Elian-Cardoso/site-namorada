namespace App\Http\Controllers;

use App\Models\Evento;
use App\Models\EventoFoto;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Storage;

class EventoController extends Controller
{
    public function index()
    {
        $eventos = Evento::with('fotos')->get();
        return view('eventos.index', compact('eventos'));
    }

    public function create()
    {
        return view('eventos.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'nome' => 'required',
            'data_evento' => 'required|date',
            'fotos.*' => 'image|mimes:jpeg,png,jpg,gif|max:2048'
        ]);

        $evento = Evento::create($request->only('nome', 'data_evento'));

        if ($request->hasFile('fotos')) {
            foreach ($request->file('fotos') as $foto) {
                $path = $foto->store('evento_fotos', 'public');
                EventoFoto::create([
                    'evento_id' => $evento->id,
                    'caminho_foto' => $path
                ]);
            }
        }

        return redirect()->route('eventos.index')->with('success', 'Evento criado com sucesso!');
    }

    public function show(Evento $evento)
    {
        $evento->load('fotos');
        return view('eventos.show', compact('evento'));
    }

    public function edit(Evento $evento)
    {
        return view('eventos.edit', compact('evento'));
    }

    public function update(Request $request, Evento $evento)
    {
        $request->validate([
            'nome' => 'required',
            'data_evento' => 'required|date'
        ]);

        $evento->update($request->only('nome', 'data_evento'));

        return redirect()->route('eventos.index')->with('success', 'Evento atualizado!');
    }

    public function destroy(Evento $evento)
    {
        // Apaga as fotos do storage
        foreach ($evento->fotos as $foto) {
            Storage::disk('public')->delete($foto->caminho_foto);
        }

        $evento->delete();

        return redirect()->route('eventos.index')->with('success', 'Evento excluído!');
    }
}
