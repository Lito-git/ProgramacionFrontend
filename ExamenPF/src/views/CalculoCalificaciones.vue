<template>
    <div class="page-container d-flex justify-content-center align-items-center">
        <form class="card-custom p-4 border rounded bg-white shadow" @submit.prevent="calcular">

            <h3 class="text-center mb-4">Cálculo de Calificaciones</h3>

            <div class="mb-3">
                <label for="nota1" class="form-label text-center d-block">Nota 1</label>
                <input id="nota1" v-model.number="nota1" type="number" min="10" max="70" class="form-control"
                    placeholder="Nota 1" required />
            </div>

            <div class="mb-3">
                <label for="nota2" class="form-label text-center d-block">Nota 2</label>
                <input id="nota2" v-model.number="nota2" type="number" min="10" max="70" class="form-control"
                    placeholder="Nota 2" required />
            </div>

            <div class="mb-3">
                <label for="nota3" class="form-label text-center d-block">Nota 3</label>
                <input id="nota3" v-model.number="nota3" type="number" min="10" max="70" class="form-control"
                    placeholder="Nota 3" required />
            </div>

            <div class="mb-3">
                <label for="asistencia" class="form-label text-center d-block">Asistencia %</label>
                <input id="asistencia" v-model.number="asistencia" type="number" min="0" max="100" class="form-control"
                    placeholder="Asistencia" required />
            </div>

            <button type="submit" class="btn btn-primary mx-auto d-block mb-3">Calcular</button>

            <!-- Mensaje de error -->
            <div v-if="!esValido" class="text-center">
                <p class="text-danger">Por favor, ingresa valores válidos para las notas y la asistencia.</p>
            </div>

            <!-- Resultados -->
            <div v-if="esValido && estaAprobado !== null" class="text-center">
                <p>El promedio es: {{ promedio }}</p>
                <p :class="{ 'text-success': estaAprobado, 'text-danger': !estaAprobado }">
                    Tu estado es: {{ estado }}
                </p>
            </div>

        </form>
    </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
    setup() {
        const nota1 = ref(null);
        const nota2 = ref(null);
        const nota3 = ref(null);
        const asistencia = ref(null);

        const promedio = ref(0);
        const estado = ref('');
        const estaAprobado = ref(null);
        const esValido = ref(true);

        const validarFormulario = () => {
            esValido.value =
                nota1.value !== null && nota1.value && nota1.value != '' && nota1.value >= 10 && nota1.value <= 70 &&
                nota2.value !== null && nota2.value && nota2.value != '' && nota2.value >= 10 && nota2.value <= 70 &&
                nota3.value !== null && nota3.value && nota3.value != '' && nota3.value >= 10 && nota3.value <= 70 &&
                asistencia.value !== null && asistencia.value !== '' && asistencia.value >= 0 && asistencia.value <= 100

            // Reiniciar resultados si el formulario es inválido
            if (!esValido.value) {
                estado.value = ''
                estaAprobado.value = null
                promedio.value = 0
            }
        }

        watch([nota1, nota2, nota3, asistencia], validarFormulario);

        const calcular = () => {
            validarFormulario();

            if (!esValido.value) return;

            promedio.value = nota1.value * 0.35 + nota2.value * 0.35 + nota3.value * 0.30;

            if (promedio.value >= 40 && asistencia.value >= 80) {
                estado.value = 'Aprobado';
                estaAprobado.value = true;

            } else {
                estado.value = 'Reprobado';
                estaAprobado.value = false;
            }
        };

        return {
            nota1,
            nota2,
            nota3,
            asistencia,
            promedio,
            estado,
            estaAprobado,
            esValido,
            calcular
        };
    }
};
</script>



<style scoped>
.card-custom {
    min-width: 500px;
    min-height: 590px;
}

.page-container {
    height: calc(100vh - 56px);
    background-color: white;
}
</style>
