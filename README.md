# 📖 App Busca Bíblica

Este é um aplicativo Android que permite ao usuário pesquisar versículos bíblicos, digitando o livro, capítulo e versículo.  
Ele utiliza a [Bible API](https://bible-api.com/) para retornar o texto da Bíblia.

> O app aceita nomes de livros digitados em **português** (ex: “joão”, “salmos”) e converte automaticamente para o formato exigido pela API (em inglês, sem acento).

---

## 📂 Arquivos principais

### ✅ `MainActivity.kt`

```kotlin
package com.example.buscabiblica

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.*
import retrofit2.*
import retrofit2.converter.gson.GsonConverterFactory
import java.text.Normalizer

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
    private fun String.normalizeText(): String {
        val normalized = Normalizer.normalize(this, Normalizer.Form.NFD)
        return Regex("\\p{InCombiningDiacriticalMarks}+").replace(normalized, "").lowercase().trim()
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Ligando os componentes do layout
        editLivro = findViewById(R.id.editLivro)
        editCapitulo = findViewById(R.id.editCapitulo)
        editVersiculo = findViewById(R.id.editVersiculo)
        btnBuscar = findViewById(R.id.btnBuscar)
        textResultado = findViewById(R.id.textResultado)

        btnBuscar.setOnClickListener {
            val livroDigitado = editLivro.text.toString().normalizeText()
            val capitulo = editCapitulo.text.toString().toIntOrNull()
            val versiculo = editVersiculo.text.toString().toIntOrNull()

            if (livroDigitado.isBlank() || capitulo == null || versiculo == null) {
                textResultado.text = "Preencha todos os campos corretamente."
                return@setOnClickListener
            }

            // Traduz o nome do livro, se existir no mapa
            val livroApi = mapaLivros[livroDigitado] ?: livroDigitado

            // Configura o Retrofit para chamada HTTP
            val retrofit = Retrofit.Builder()
                .baseUrl("https://bible-api.com/")
                .addConverterFactory(GsonConverterFactory.create())
                .build()

            val api = retrofit.create(BibleApi::class.java)

            // Faz a requisição com Retrofit
            val call = api.getVersiculo(livroApi, capitulo, versiculo)
            call.enqueue(object : Callback<BibleResponse> {
                override fun onResponse(
                    call: Call<BibleResponse>,
                    response: Response<BibleResponse>
                ) {
                    if (response.isSuccessful) {
                        val versiculo = response.body()
                        if (versiculo != null && versiculo.text.isNotEmpty()) {
                            textResultado.text = "${versiculo.reference}\n\n${versiculo.text}"
                        } else {
                            textResultado.text = "Versículo não encontrado!"
                        }
                    } else {
                        textResultado.text = "Versículo não encontrado!"
                    }
                }

                override fun onFailure(call: Call<BibleResponse>, t: Throwable) {
                    textResultado.text = "Erro na conexão: ${t.message}"
                }
            })
        }
    }
}
```

### ✅ `IbgeApi.kt`
```kotlin
package com.example.buscabiblica

import retrofit2.Call
import retrofit2.http.GET
import retrofit2.http.Path

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
### ✅ `BibleResponse.kt`
```kotlin
package com.example.buscabiblica

// Modelo de dados da resposta da Bible API
data class BibleResponse(
    val reference: String,        // Ex: "John 3:16"
    val text: String,             // Texto do versículo
    val translation_name: String // Nome da tradução
)
```
##✅ Exemplo de uso

Se o usuário digitar:

    Livro: João

    Capítulo: 3

    Versículo: 16

O app faz a busca para https://bible-api.com/john%203:16 e retorna:

John 3:16

For God so loved the world that he gave his one and only Son, 
that whoever believes in him shall not perish but have eternal life.

ℹ️ Observações

    O app aceita nomes em português com acento, mas usa um sistema de normalização e tradução para converter o nome corretamente.

    O campo text pode vir em inglês (a API atualmente só oferece esse idioma).

    A API usada é gratuita e pública: https://bible-api.com

...


