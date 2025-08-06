# 📱 IBGE Nomes App

Este app Android consulta a frequência de nomes próprios no Brasil usando a [API de nomes do IBGE](https://servicodados.ibge.gov.br/api/docs/nomes).  
Ele retorna a frequência do nome digitado por década de nascimento.

Construído com **Kotlin** + **Retrofit**.

---

## 📂 Arquivos principais

### ✅ `MainActivity.kt`

```kotlin
package com.example.ibgenomesapp

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.*
import retrofit2.*
import retrofit2.converter.gson.GsonConverterFactory

class MainActivity : AppCompatActivity() {

    // Elementos da interface
    lateinit var editNome: EditText
    lateinit var btnBuscar: Button
    lateinit var textResultado: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Ligando os componentes da tela
        editNome = findViewById(R.id.editNome)
        btnBuscar = findViewById(R.id.btnBuscar)
        textResultado = findViewById(R.id.textResultado)

        // Criando o Retrofit (cliente HTTP)
        val retrofit = Retrofit.Builder()
            .baseUrl("https://servicodados.ibge.gov.br/api/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()

        // Criando a instância da interface da API
        val api = retrofit.create(IbgeApi::class.java)

        // Ação ao clicar no botão
        btnBuscar.setOnClickListener {
            val nome = editNome.text.toString().trim().lowercase()

            if (nome.isBlank()) {
                textResultado.text = "Por favor, digite um nome."
                return@setOnClickListener
            }

            val call = api.getNameInfo(nome)

            // Fazendo a requisição
            call.enqueue(object : Callback<List<IbgeResponse>> {
                override fun onResponse(
                    call: Call<List<IbgeResponse>>,
                    response: Response<List<IbgeResponse>>
                ) {
                    if (response.isSuccessful) {
                        val lista = response.body()
                        if (lista != null && lista.isNotEmpty()) {
                            val info = lista[0]
                            val sb = StringBuilder()
                            sb.append("Nome: ${info.nome.uppercase()}\n\n")
                            sb.append("Frequência por década:\n")

                            val resList = info.res
                            if (resList.isNullOrEmpty()) {
                                sb.append("Sem dados de frequências disponíveis.")
                            } else {
                                resList.forEach {
                                    sb.append("${it.periodo}: ${it.frequencia}\n")
                                }
                            }

                            textResultado.text = sb.toString()
                        } else {
                            textResultado.text = "Nome não encontrado."
                        }
                    } else {
                        textResultado.text = "Erro na resposta da API."
                    }
                }

                override fun onFailure(call: Call<List<IbgeResponse>>, t: Throwable) {
                    textResultado.text = "Erro na conexão: ${t.message}"
                }
            })
        }
    }
}
```

### ✅ `IbgeApi.kt`
```kotlin
package com.example.ibgenomesapp

import retrofit2.Call
import retrofit2.http.GET
import retrofit2.http.Path

// Interface usada pelo Retrofit para acessar a API do IBGE
interface IbgeApi {
    @GET("v2/censos/nomes/{nome}")
    fun getNameInfo(@Path("nome") nome: String): Call<List<IbgeResponse>>
}
```
### ✅ `BibleResponse.kt`
```kotlin
package com.example.buscabiblica

data class BibleResponse(
    val reference: String,
    val text: String,
    val translation_name: String
)
```

ℹ️ Observações

    O campo "frequência" indica quantas pessoas receberam aquele nome naquela década.

    A API retorna os dados a partir do Censo Nacional, ou seja, dados reais e atualizados pelo IBGE.

    A pesquisa diferencia maiúsculas/minúsculas, então usamos lowercase() para garantir o funcionamento.

## ✅ Exemplo de uso

Se você digitar maria, o app pode exibir algo como:

Nome: MARIA

Frequência por década:
[1930,1940[: 749053
[1940,1950[: 1487042
[1950,1960[: 2476482
[1960,1970[: 2495491
[1970,1980[: 1616019
...


