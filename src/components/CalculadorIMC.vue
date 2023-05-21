<template>
   <div class="container">
      <h1 class="title">Calculadora de IMC</h1>
      <img src="../assets/logo.png" alt="Logo da Calculadora" class="logo">
      <div class="form-group">
         <label for="peso" class="label">Peso (em kg):</label>
         <input type="number" id="peso" v-model="pessoa.peso" class="input-field" min="0" placeholder="Ex: 60.5" />
         <p v-if="pesoError" class="error-message"><strong> {{ pesoError }}</strong> </p>
      </div>
      <div class="form-group">
         <label for="altura" class="label">Altura (em cm):</label>
         <input type="number" id="altura" v-model="pessoa.altura" class="input-field" min="0" placeholder="Ex: 170" />
         <p v-if="alturaError" class="error-message"><strong> {{ alturaError }} </strong></p>
      </div>
      <button @click="calcularIMC" class="calculate-button">Calcular</button>
      <div v-if="pessoa.resultado" class="result">
         <p>Seu IMC é: {{ pessoa.resultado }}</p>
         <p>Sua categoria é: {{ pessoa.categoria }} </p>
      </div>
   </div>
</template>

<script>
export default {
   data() {
      return {
         pessoa: {
            peso: null,
            altura: null,
            resultado: null,
            categoria: null,
         },
         categoriasIMC: [
            {
               categoria: "Abaixo do peso",
               limiteInferior: 0,
               limiteSuperior: 18.5,
            },
            {
               categoria: "Peso normal",
               limiteInferior: 18.5,
               limiteSuperior: 24.9,
            },
            {
               categoria: "Sobrepeso",
               limiteInferior: 25.0,
               limiteSuperior: 29.9,
            },
            {
               categoria: "Obesidade (classe 1)",
               limiteInferior: 30.0,
               limiteSuperior: 34.9,
            },
            {
               categoria: "Obesidade (classe 2)",
               limiteInferior: 35.0,
               limiteSuperior: 39.9,
            },
            {
               categoria: "Obesidade grave (classe 3)",
               limiteInferior: 40.0,
               limiteSuperior: 999,
            },
         ],
         pesoError: null,
         alturaError: null,
      };
   },
   methods: {
      calcularIMC() {
         this.pesoError = null;
         this.alturaError = null;
         this.pessoa.resultado = null;
         this.pessoa.categoria = null;

         if (!this.pessoa.peso || this.pessoa.peso <= 0) {
            this.pesoError = "Peso inválido";
         }

         if (!this.pessoa.altura || this.pessoa.altura <= 0) {
            this.alturaError = "Altura inválida";
         }

         if (!this.pesoError && !this.alturaError) {
            const alturaEmMetros = this.pessoa.altura / 100;
            const imc = this.pessoa.peso / Math.pow(alturaEmMetros, 2);
            this.pessoa.resultado = imc.toFixed(2);

            this.categoriasIMC.forEach((categoria) => {
               if (this.pessoa.resultado >= categoria.limiteInferior && this.pessoa.resultado <= categoria.limiteSuperior) {
                  this.pessoa.categoria = categoria.categoria;
               }
            });
         }
      },
   },
};
</script>
 
<style>
.container {
   max-width: 400px;
   margin: auto;
   padding: 20px;
   background-color: #f5f5f5;
   border-radius: 8px;
   text-align: center;
}

.title {
   font-size: 24px;
   color: #333;
   margin-bottom: 20px;
}

.logo {
   display: block;
   margin: 0 auto;
   width: 150px;
   height: auto;
   margin-bottom: 20px;
}

.form-group {
   margin-bottom: 10px;
}

.label {
   display: inline-block;
   text-align: right;
   font-size: 16px;
   color: #333;
   width: 120px;
   margin-right: 10px;
}

.input-field {
   display: inline-block;
   width: 200px;
   padding: 8px;
   border: 1px solid #ccc;
   border-radius: 4px;
   font-size: 15px;
}

.calculate-button {
   width: 30%;
   padding: 10px;
   background-color: #5900f4;
   color: #fff;
   border: none;
   border-radius: 4px;
   cursor: pointer;
   margin-top: 10px;
}

.result {
   margin-top: 20px;
   font-size: 18px;
   color: #333;
}

p {
   font-size: 16px;
}

.error-message {
   font-size: 14px;
   color: red;
}

</style>
 