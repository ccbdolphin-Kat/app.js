# app.js
// Base de datos local integrada directamente (sin requerir internet)
const documentoIne = [
    { titulo: "Artículo 1. Ámbito de aplicación", categoria: "Disposiciones Generales", texto: "Las disposiciones del presente Código son aplicables a todas las personas servidoras públicas que desempeñen un empleo, cargo o comisión en el Instituto..." },
    { titulo: "Artículo 3. Glosario - Inteligencia Artificial", categoria: "Glosario", texto: "Disciplina tecnológica que desarrolla modelos, métodos y sistemas informáticos capaces de imitar, ampliar o automatizar funciones de la inteligencia humana..." },
    { titulo: "Artículo 3. Glosario - Conflicto de Interés", categoria: "Glosario", texto: "La posible afectación del desempeño imparcial y objetivo de las funciones de las personas servidoras públicas en razón de intereses personales, familiares o de negocios." },
    { titulo: "Artículo 4. Principio de Certeza", categoria: "Principios", texto: "Los actos y acciones de las personas servidoras públicas deberán ser previsibles y sustentarse en información veraz y comprobable, a fin de que los resultados de sus actividades sean verificables..." },
    { titulo: "Artículo 4. Principio de Legalidad", categoria: "Principios", texto: "Las personas servidoras públicas deberán actuar con estricto apego al marco constitucional y legal, ejercer sus atribuciones únicamente en los términos que la ley les confiere..." },
    { titulo: "Artículo 6. Fracción XIX. Uso ético de la IA", categoria: "Reglas de Integridad", texto: "Asegurar supervisión humana en las decisiones relevantes. Verificar la calidad, pertinencia y licitud de los datos utilizados... No incorporar información reservada en herramientas externas." }
];

const buscador = document.getElementById('buscador');
const contenedor = document.getElementById('contenido');

// Función para renderizar tarjetas en pantalla
function mostrarDatos(datos) {
    contenedor.innerHTML = '';
    if(datos.length === 0) {
        contenedor.innerHTML = '<p style="text-align:center; color:#999;">No se encontraron resultados.</p>';
        return;
    }
    datos.forEach(item => {
        contenedor.innerHTML += `
            <div class="card">
                <span class="badge">${item.categoria}</span>
                <h3>${item.titulo}</h3>
                <p>${item.texto}</p>
            </div>
        `;
    });
}

// Escuchar lo que escribe el usuario para filtrar en tiempo real
buscador.addEventListener('input', (e) => {
    const termino = e.target.value.toLowerCase();
    const filtrados = documentoIne.filter(item => 
        item.titulo.toLowerCase().includes(termino) || 
        item.texto.toLowerCase().includes(termino) ||
        item.categoria.toLowerCase().includes(termino)
    );
    mostrarDatos(filtrados);
});

// Carga inicial de la app
mostrarDatos(documentoIne);

// Registro Obligatorio del Service Worker para funcionamiento Sin Internet
if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        navigator.serviceWorker.register('sw.js')
            .then(() => console.log('App lista para usarse sin internet.'))
            .catch(err => console.log('Error de registro offline:', err));
    });
}
