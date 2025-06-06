<?php

namespace App\Http\Controllers;

use App\Models\Loja;
use Illuminate\Http\Request;

class LojaController extends Controller
{
    public function index()
    {
        $lojas = Loja::latest()->get();
        return view('lojas.index', compact('lojas'));
    }

    public function create()
    {
        return view('lojas.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'nome' => 'required',
            'link' => 'required|url',
        ]);

        Loja::create($request->all());

        return redirect()->route('lojas.index')->with('success', 'Loja parceira adicionada com sucesso.');
    }

    public function show(Loja $loja)
    {
        return view('lojas.show', compact('loja'));
    }

    public function edit(Loja $loja)
    {
        return view('lojas.edit', compact('loja'));
    }

    public function update(Request $request, Loja $loja)
    {
        $request->validate([
            'nome' => 'required',
            'link' => 'required|url',
        ]);

        $loja->update($request->all());

        return redirect()->route('lojas.index')->with('success', 'Loja atualizada com sucesso.');
    }

    public function destroy(Loja $loja)
    {
        $loja->delete();
        return redirect()->route('lojas.index')->with('success', 'Loja removida com sucesso.');
    }
}
