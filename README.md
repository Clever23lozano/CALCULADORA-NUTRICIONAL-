<html lang="es"> 
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <title>Registro e Inicio de Sesión</title> 
    <style>
        /* Estilos para el header */
        header {
            background-color: #1abc9c; /* Color de fondo */
            color: #fff; /* Color del texto */
            text-align: center; /* Alineación del texto */
            padding: 20px 0; /* Espaciado interno */
            margin-bottom: 20px; /* Margen inferior */
            font-family: 'Arial Black', sans-serif; /* Fuente */
        }

        /* Estilos para el título */
        header h1 {
            font-size: 36px; /* Tamaño de la fuente */
            margin: 0; /* Eliminar margen */
            text-transform: uppercase; /* Convertir texto a mayúsculas */
            letter-spacing: 2px; /* Espaciado entre letras */
        }

        /* Estilos para el contenido */
        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
            text-align: center;
            margin: 0 auto; /* Centrar el contenedor horizontalmente */
            font-family: Arial, sans-serif; /* Fuente */
        }

        /* Estilos para los botones */
        button {
            background-color: #3498db; /* Color de fondo */
            color: #fff; /* Color del texto */
            padding: 10px 20px; /* Espaciado interno */
            border: none; /* Sin borde */
            border-radius: 5px; /* Bordes redondeados */
            cursor: pointer; /* Cursor al pasar sobre el botón */
            margin-top: 10px; /* Margen superior */
            width: 100%; /* Ancho completo */
            font-size: 16px; /* Tamaño de la fuente */
            transition: background-color 0.3s ease; /* Transición suave del color de fondo */
        }

        button:hover {
            background-color: #2980b9; /* Color de fondo al pasar el cursor */
        }

        /* Estilos para los enlaces */
        a {
            color: #3498db; /* Color del enlace */
            text-decoration: none; /* Sin subrayado */
            display: inline-block; /* Mostrar como bloque en línea */
            margin-top: 10px; /* Margen superior */
            font-size: 14px; /* Tamaño de la fuente */
        }

        a:hover {
            text-decoration: underline; /* Subrayado al pasar el cursor */
        }

        /* Estilos para el pie de página */
        .footer {
            margin-top: 20px; /* Margen superior */
            color: #666; /* Color del texto */
            font-size: 12px; /* Tamaño de la fuente */
        }

        /* Estilos para los formularios */
        .container form {
            margin-bottom: 20px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            padding: 20px;
        }

        .container form input[type="text"],
        .container form input[type="email"],
        .container form input[type="password"] {
            width: calc(100% - 24px);
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        .container form button {
            background-color: #3498db;
            color: #fff;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            font-size: 16px;
            transition: background-color 0.3s ease;
        }

        .container form button:hover {
            background-color: #2980b9;
        }
    </style> 
</head> 
<body> 
    <header>
        <h1>CALCULADORA-NUTRICIONAL</h1>
    </header>

    <div class="container"> 
        <form id="registroForm" onsubmit="registrarUsuario(event)"> 
            <h1>Registro</h1>
            <label for="nombreUsuario">Nombre de Usuario:</label> 
            <input type="text" id="nombreUsuario" name="nombreUsuario" required>
            <br> 
            <label for="correo">Correo Electrónico:</label> 
            <input type="email" id="correo" name="correo" required>
            <br> 
            <label for="contrasena">Contraseña:</label> 
            <input type="password" id="contrasena" name="contrasena" required>
            <br> 
            <label for="confirmarContrasena">Confirmar Contraseña:</label> 
            <input type="password" id="confirmarContrasena" name="confirmarContrasena" required>
            <br>
            <input type="checkbox" onclick="mostrarContrasena()"> Mostrar contraseña <!-- Checkbox para mostrar/ocultar la contraseña -->
            <br>
            <button type="submit">Registrarse</button> 
            <button type="button" onclick="registrarConGoogle()">Registrarse con Google</button> 
        </form> 

        <form id="inicioSesionForm"> 
            <h1>Iniciar Sesión</h1> 
            <label for="correoInicio">Correo Electrónico:</label> 
            <input type="email" id="correoInicio" name="correoInicio" required>
            <br> 
            <label for="contrasenaInicio">Contraseña:</label> 
            <input type="password" id="contrasenaInicio" name="contrasenaInicio" required>
            <input type="checkbox" onclick="mostrarContrasenaInicio()"> Mostrar contraseña <!-- Checkbox para mostrar/ocultar la contraseña -->
            <br> 
            <button type="button" onclick="iniciarSesion()">Iniciar Sesión</button> 
            <a href="#" onclick="recuperarContrasena()">¿Olvidaste tu contraseña?</a> 
        </form> 
    <p class="footer">© 2024 Calculadora Nutricional. Todos los derechos reservados.</p> 
</div> 

<!-- Incluir Firebase mediante etiquetas script --> 
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script> 
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-auth.js"></script> 
<script>
    var firebaseConfig = {
        apiKey: "AIzaSyBAyn2xWuA8kv0w0WcqFdkBMHMNf2IBq00",
        authDomain: "calculadora-nutricional-ff61e.firebaseapp.com",
        projectId: "calculadora-nutricional-ff61e"
    };

    // Inicializar Firebase
    firebase.initializeApp(firebaseConfig);

    // Función para validar la contraseña
    function validarContrasena(contrasena) {
        // Verificar si la contraseña cumple con los requisitos deseados
        // Por ejemplo, al menos una mayúscula, una minúscula, un número y longitud mínima
        var regex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/;
        return regex.test(contrasena);
    }

    // Función para validar el formato del correo electrónico
    function validarFormatoCorreo(correo) {
        // Verificar si el correo electrónico tiene el formato deseado
        var regex = /^[a-zA-Z0-9._%+-]+@(gmail|hotmail|yahoo)\.(com|es|net)$/;
        return regex.test(correo);
    }

    // Función para registrar un usuario
    function registrarUsuario(event) {
        event.preventDefault(); // Evitar que el formulario se envíe automáticamente al presionar el botón "Registrarse".
        // Obtener valores del formulario
        var nombreUsuario = document.getElementById("nombreUsuario").value;
        var correo = document.getElementById("correo").value;
        var contrasena = document.getElementById("contrasena").value;
        var confirmarContrasena = document.getElementById("confirmarContrasena").value;

        // Verificar si las contraseñas coinciden
        if (contrasena !== confirmarContrasena) {
            alert("Las contraseñas no coinciden.");
            return; // Detener el proceso de registro
        }

        // Verificar si la contraseña es válida
        if (!validarContrasena(contrasena)) {
            // Mostrar mensaje de alerta si la contraseña no cumple con los requisitos
            alert("La contraseña debe contener al menos una mayúscula, una minúscula, un número y tener una longitud mínima de 8 caracteres.");
            return; // Detener el proceso de registro
        }

        // Verificar si el correo electrónico tiene el formato deseado
        if (!validarFormatoCorreo(correo)) {
            // Mostrar mensaje de alerta si el formato del correo electrónico no es válido
            alert("El correo electrónico debe tener el formato correcto: @gmail.com, @hotmail.com, @yahoo.com, etc.");
            return; // Detener el proceso de registro
        }

        // Crear cuenta de usuario con correo y contraseña
        firebase.auth().createUserWithEmailAndPassword(correo, contrasena)
        .then((userCredential) => { 
          // Usuario registrado correctamente
            var user = userCredential.user;
            console.log("Usuario registrado:", user);
            alert("¡Registro exitoso!");
        })
        .catch((error) => {
            // Error al registrar usuario
            var errorCode = error.code;
            var errorMessage = error.message;
            console.error("Error:", errorMessage);
            alert("Error al registrar usuario: " + errorMessage);
        });
    }

    // Función para recuperar contraseña
    function recuperarContrasena() {
        var correoRecuperacion = prompt("Ingresa tu correo electrónico para recuperar tu contraseña:");
        
        if (correoRecuperacion) {
            firebase.auth().sendPasswordResetEmail(correoRecuperacion)
            .then(() => {
                // Email de recuperación enviado
                alert("Se ha enviado un correo electrónico de recuperación de contraseña a " + correoRecuperacion);
            })
            .catch((error) => {
                // Error al enviar email de recuperación
                var errorMessage = "Error al enviar correo electrónico de recuperación. Por favor, intenta nuevamente.";
                console.error("Error:", error);
                alert(errorMessage);
            });
        }
    }

    // Función para mostrar/ocultar la contraseña de registro
    function mostrarContrasena() {
        var contrasenaInput = document.getElementById("contrasena");
        var confirmarContrasenaInput = document.getElementById("confirmarContrasena");
        if (contrasenaInput.type === "password") {
            contrasenaInput.type = "text";
            confirmarContrasenaInput.type = "text";
        } else {
            contrasenaInput.type = "password";
            confirmarContrasenaInput.type = "password";
        }
    }

    // Función para mostrar/ocultar la contraseña de inicio de sesión
    function mostrarContrasenaInicio() {
        var contrasenaInput = document.getElementById("contrasenaInicio");
        if (contrasenaInput.type === "password") {
            contrasenaInput.type = "text";
        } else {
            contrasenaInput.type = "password";
        }
    }

    // Función para iniciar sesión
    function iniciarSesion() {
        var correo = document.getElementById("correoInicio").value;
        var contrasena = document.getElementById("contrasenaInicio").value;

        firebase.auth().signInWithEmailAndPassword(correo, contrasena)
        .then((userCredential) => {
            // Usuario inició sesión correctamente
            var user = userCredential.user;
            console.log("Usuario inició sesión:", user);
            alert("¡Inicio de sesión exitoso!");
            // Redireccionar a la página de bienvenida
            window.location.href = "https://clever23lozano.github.io/CALCULADORA-NUTRICIONAL/bienvenida.html";
        })
        .catch((error) => {
            // Error al iniciar sesión
            var errorCode = error.code;
            var errorMessage = error.message;
            console.error("Error:", errorMessage);
            alert("Error al iniciar sesión: " + errorMessage);
        });
    }
</script>
</body>
</html>
