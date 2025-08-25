<template>
    <div class="page-container d-flex justify-content-center align-items-center">
        <form class="card-custom p-4 border rounded bg-white shadow" @submit.prevent="enviarForm">

            <h3 class="text-center mb-4">Formulario de Registro</h3>

            <div class="mb-3">
                <label for="nombre" class="form-label text-center d-block">Nombre</label>
                <input id="nombre" v-model="form.nombre" type="text" class="form-control" placeholder="Nombre"
                    @input="validarNombre" />
                <p class="text-danger text-center">{{ errors.nombreError }}</p>
            </div>

            <div class="mb-3">
                <label for="correo" class="form-label text-center d-block">Correo</label>
                <input id="correo" v-model="form.email" type="text" class="form-control" placeholder="Correo"
                    @input="validarEmail" />
                <p class="text-danger text-center">{{ errors.emailError }}</p>
            </div>

            <div class="mb-3">
                <label for="contrasenia" class="form-label text-center d-block">Contraseña</label>
                <input id="contrasenia" v-model="form.contrasenia" type="password" class="form-control"
                    placeholder="Contraseña" @input="validarContrasenia" />
                <p class="text-danger text-center">{{ errors.contraseniaError }}</p>
            </div>

            <div class="mb-3">
                <label for="repetirContrasenia" class="form-label text-center d-block">Repetir Contraseña</label>
                <input id="repetirContrasenia" v-model="form.repetirContrasenia" type="password" class="form-control"
                    placeholder="Repetir Contraseña" @input="validarRepetirContrasenia" />
                <p class="text-danger text-center">{{ errors.repetirContraseniaError }}</p>
            </div>

            <button type="submit" class="btn btn-primary mx-auto d-block mb-3">Enviar</button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: {
                nombre: '',
                email: '',
                contrasenia: '',
                repetirContrasenia: ''
            },
            errors: {
                nombreError: '',
                emailError: '',
                contraseniaError: '',
                repetirContraseniaError: ''
            }
        };
    },
    methods: {
        validarNombre() {
            this.errors.nombreError = '';

            if (!this.form.nombre) {
                this.errors.nombreError = 'El campo nombre es requerido';
            } else if (/\d/.test(this.form.nombre)) {
                this.errors.nombreError = 'El nombre no debe contener números';
            }
        },
        validarEmail() {
            this.errors.emailError = '';

            if (!this.form.email) {
                this.errors.emailError = 'El campo email es requerido';

            } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.form.email)) {
                this.errors.emailError = 'El correo es inválido';
            }
        },
        validarContrasenia() {
            this.errors.contraseniaError = '';

            if (!this.form.contrasenia) {
                this.errors.contraseniaError = 'El campo contraseña es requerido';

            } else if (this.form.contrasenia.length < 7) {
                this.errors.contraseniaError = 'La contraseña debe tener al menos 7 caracteres';
            }
        },
        validarRepetirContrasenia() {
            this.errors.repetirContraseniaError = '';

            if (!this.form.repetirContrasenia) {
                this.errors.repetirContraseniaError = 'El campo repetir contraseña es requerido';

            } else if (this.form.contrasenia && this.form.repetirContrasenia &&
                this.form.contrasenia !== this.form.repetirContrasenia) {
                this.errors.repetirContraseniaError = 'Las contraseñas no coinciden';
            }
        },
        validarFormulario() {
            this.validarNombre();
            this.validarEmail();
            this.validarContrasenia();
            this.validarRepetirContrasenia();

            return !this.errors.nombreError &&
                !this.errors.emailError &&
                !this.errors.contraseniaError &&
                !this.errors.repetirContraseniaError;
        },
        enviarForm() {
            const esValido = this.validarFormulario();

            if (!esValido) return;

            alert('El registro se ha realizado correctamente');

            this.form.nombre = '';
            this.form.email = '';
            this.form.contrasenia = '';
            this.form.repetirContrasenia = '';
        }
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

@media (max-width: 576px) {
    .card-custom {
        min-width: 90%;
        min-height: auto;
    }

    .page-container {
        height: auto;
        padding: 20px 0;
        margin-top: 5rem;
    }

}
</style>
