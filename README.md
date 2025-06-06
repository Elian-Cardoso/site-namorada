public function up(): void
{
    Schema::create('evento_fotos', function (Blueprint $table) {
        $table->id();
        $table->foreignId('evento_id')->constrained()->onDelete('cascade');
        $table->string('caminho_foto');
        $table->timestamps();
    });
}
