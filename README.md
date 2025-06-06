<?php

namespace App\Http\Controllers;

use App\Models\Evento;
use App\Models\EventoFoto;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Storage;

class EventoController extends Controller
{
    public function index()
    {
        $eventos = Evento::with('fotos')->latest()->get();
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
            'fotos.*' => 'image|mimes:jpeg,png,jpg,gif,svg|max:2048',
        ]);

        $evento = Evento::create([
            'nome' => $request->nome,
            'data_evento' => $request->data_evento,
        ]);

        if ($request->hasFile('fotos')) {
            foreach ($request->file('fotos') as $foto) {
                $caminho = $foto->store('eventos', 'public');
                EventoFoto::create([
                    'evento_id' => $evento->id,
                    'caminho_foto' => $caminho,
                ]);
            }
        }

        return redirect()->route('eventos.index')->with('success', 'Evento criado com sucesso.');
    }

    public function show(Evento $evento)
    {
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
            'data_evento' => 'required|date',
        ]);

        $evento->update([
            'nome' => $request->nome,
            'data_evento' => $request->data_evento,
        ]);

        return redirect()->route('eventos.index')->with('success', 'Evento atualizado com sucesso.');
    }

    public function destroy(Evento $evento)
    {
        // Apaga fotos do storage
        foreach ($evento->fotos as $foto) {
            Storage::disk('public')->delete($foto->caminho_foto);
        }

        $evento->delete();

        return redirect()->route('eventos.index')->with('success', 'Evento removido com sucesso.');
    }
}
