<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Botón de Copiado con Título y Texto Diferente</title>
    <style>
        /* --- Estilos CSS para el botón --- */
        .btn-copiar {
            background-color: #007bff; /* Azul vibrante */
            color: white; /* Texto blanco para contraste */
            padding: 12px 25px; /* Espaciado generoso */
            border: none; /* Sin borde */
            border-radius: 8px; /* Bordes suavemente redondeados */
            cursor: pointer; /* Indica que es clickable */
            font-size: 17px; /* Tamaño de fuente legible */
            font-weight: bold; /* Texto en negrita */
            transition: background-color 0.3s ease, transform 0.1s ease; /* Transiciones suaves */
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Sutil sombra para profundidad */
            display: inline-flex; /* Permite alinear iconos si los hubiera */
            flex-direction: column; /* Organiza el texto interno del botón en columna */
            align-items: center; /* Centra el contenido horizontalmente dentro del botón */
            justify-content: center; /* Centra el contenido verticalmente dentro del botón */
            text-align: center; /* Asegura que el texto dentro del botón también se centre */
            gap: 5px; /* Espacio entre las líneas de texto (opcional) */

            /* Para apilar los botones verticalmente */
            display: block; /* Cada botón ocupará su propia línea */
            margin-bottom: 15px; /* Espacio entre botones apilados */
            width: fit-content; /* El botón solo ocupará el ancho necesario para su contenido */
            max-width: 90%; /* Evita que el botón sea demasiado ancho en pantallas pequeñas */
        }

        .btn-copiar:hover {
            background-color: #0056b3; /* Un azul más oscuro al pasar el ratón */
            transform: translateY(-2px); /* Pequeño efecto de elevación */
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3); /* Sombra más pronunciada */
        }

        .btn-copiar:active {
            background-color: #004085; /* Azul aún más oscuro al hacer clic */
            transform: translateY(0); /* Vuelve a su posición original */
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); /* Sombra más suave al hacer clic */
        }

        /* --- Estilos adicionales para la página (solo para demostración) --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f0f2f5; /* Fondo suave */
            color: #333;
        }

        h1 {
            color: #2c3e50;
            margin-bottom: 30px;
        }

        .contenedor-botones {
            display: flex;
            flex-direction: column; /* Apila los elementos dentro de este contenedor */
            align-items: center; /* Centra los botones individualmente */
            gap: 15px; /* Espacio entre los botones apilados */
        }

        .mensaje-copiado {
            margin-top: 20px;
            padding: 10px 20px;
            background-color: #28a745; /* Verde para éxito */
            color: white;
            border-radius: 5px;
            opacity: 0; /* Inicialmente invisible */
            transition: opacity 0.3s ease-in-out;
            text-align: center;
        }

        .mensaje-copiado.mostrar {
            opacity: 1; /* Se hace visible */
        }
    </style>
</head>
<body>

    <h1>CUENTAS VIGENTE DE CAJA BELU</h1>

    <div class="contenedor-botones">
        <button id="botonCopiadorUno" class="btn-copiar" data-texto-a-copiar="0000003100084387553290">
            Copiar CBU <br>
            Nicolas Gudiño Benítez
        </button>

        <button id="botonCopiadorDos" class="btn-copiar" data-texto-a-copiar="0000003100070078771203">
            Copiar CBU <br>
            Matias Ledesma 

        </button>

        <button id="botonCopiadorTres" class="btn-copiar" data-texto-a-copiar="NO DISPONIBLE">
            Copiar <br>
            CUENTA DNI
        </button>
    </div>

    <div id="mensajeCopiado" class="mensaje-copiado"></div>

    <script>
        /**
         * Función para configurar un botón que copia texto al portapapeles.
         * @param {string} idBoton El ID del elemento botón.
         * @param {string} [textoACopiarDirecto] (Opcional) El texto a copiar. Si se proporciona,
         * tiene prioridad sobre el atributo data-texto-a-copiar.
         */
        async function configurarBotonCopiador(idBoton, textoACopiarDirecto) {
            const boton = document.getElementById(idBoton);
            const mensajeCopiado = document.getElementById('mensajeCopiado');

            if (!boton) {
                console.error(`Error: El botón con ID "${idBoton}" no fue encontrado en el DOM.`);
                return;
            }

            // Determinar el texto final a copiar:
            // 1. Si se pasa directamente a la función (`textoACopiarDirecto`).
            // 2. Si está en el atributo `data-texto-a-copiar` del botón (¡esta es la clave para tu pregunta!).
            // 3. Como última opción (fallback), usa el texto visible del botón (`boton.textContent`).
            const textoFinalACopiar = textoACopiarDirecto || boton.dataset.textoACopiar || boton.textContent;

            if (!textoFinalACopiar || textoFinalACopiar.trim() === '') {
                console.warn(`Advertencia: No se ha especificado ningún texto válido para copiar para el botón con ID "${idBoton}".`);
                return;
            }

            boton.addEventListener('click', async () => {
                try {
                    await navigator.clipboard.writeText(textoFinalACopiar);
                    
                    // Muestra el mensaje de éxito
                    mensajeCopiado.textContent = `¡Copiado: "${textoFinalACopiar}"!`; // Mostrar el texto que realmente se copió
                    mensajeCopiado.classList.add('mostrar');

                    // Oculta el mensaje después de 2 segundos
                    setTimeout(() => {
                        mensajeCopiado.classList.remove('mostrar');
                    }, 2000);

                } catch (err) {
                    console.error('Error al intentar copiar el texto:', err);
                    alert('¡Ups! No se pudo copiar el texto. Asegúrate de que tu navegador lo permita (a menudo requiere HTTPS).');
                }
            });
        }

        // --- Configuración de los botones al cargar la página ---
        document.addEventListener('DOMContentLoaded', () => {
            // Este botón copiará el texto visible (porque no tiene data-texto-a-copiar ni se le pasa texto directo)
            configurarBotonCopiador('botonCopiadorUno');

            // Este botón tiene un título visible diferente al texto que copiará (definido en data-texto-a-copiar)
            configurarBotonCopiador('botonCopiadorDos');

            // Otro ejemplo con data-texto-a-copiar
            configurarBotonCopiador('botonCopiadorTres');
        });

    </script>
</body>
</html>
