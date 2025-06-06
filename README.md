public function up(): void
{
    Schema::create('reviews', function (Blueprint $table) {
        $table->id();
        $table->string('email');
        $table->tinyInteger('avaliacao'); // 1 a 5 estrelas
        $table->text('comentario');
        $table->timestamps();
    });
}
