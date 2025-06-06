public function up(): void
{
    Schema::create('lojas', function (Blueprint $table) {
        $table->id();
        $table->string('nome');
        $table->string('link');
        $table->timestamps();
    });
}
