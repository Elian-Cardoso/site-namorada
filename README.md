public function up(): void
{
    Schema::create('eventos', function (Blueprint $table) {
        $table->id();
        $table->string('nome');
        $table->date('data_evento');
        $table->timestamps();
    });
}
