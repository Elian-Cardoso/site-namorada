<?php

namespace App\Http\Controllers;

use App\Models\Review;
use Illuminate\Http\Request;

class ReviewController extends Controller
{
    public function index()
    {
        $reviews = Review::latest()->get();
        return view('reviews.index', compact('reviews'));
    }

    public function create()
    {
        return view('reviews.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'email' => 'required|email',
            'estrelas' => 'required|integer|min:1|max:5',
            'comentario' => 'required|string',
        ]);

        Review::create($request->all());

        return redirect()->route('reviews.index')->with('success', 'Review enviada com sucesso!');
    }

    public function show(Review $review)
    {
        return view('reviews.show', compact('review'));
    }

    public function edit(Review $review)
    {
        return view('reviews.edit', compact('review'));
    }

    public function update(Request $request, Review $review)
    {
        $request->validate([
            'email' => 'required|email',
            'estrelas' => 'required|integer|min:1|max:5',
            'comentario' => 'required|string',
        ]);

        $review->update($request->all());

        return redirect()->route('reviews.index')->with('success', 'Review atualizada com sucesso!');
    }

    public function destroy(Review $review)
    {
        $review->delete();
        return redirect()->route('reviews.index')->with('success', 'Review removida com sucesso!');
    }
}
