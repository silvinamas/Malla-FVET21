<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Malla Curricular Interactiva - Veterinaria</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f4f4f4; padding: 2rem; }
    h1 { text-align: center; }
    h2 { background-color: #005a87; color: white; padding: 0.5rem; border-radius: 5px; }
    .materia { padding: 0.3rem 0.5rem; margin: 0.2rem 0; background: white; border-radius: 5px; display: flex; align-items: center; }
    .materia input { margin-right: 10px; }
    .bloqueada { color: gray; }
    .aprobada { text-decoration: ; color: green; }
    .semestre { margin-bottom: 2rem; }
  </style>
</head>
<body>
  <h1>Malla Curricular Interactiva</h1>
  <div id="contenedor"></div>

  <script>
    const estructura = {
      "Primer Año - Primer Semestre": ["BQD", "AS", "CHG", "BF", "IEV"],
      "Primer Año - Segundo Semestre": ["ETNO", "HSE", "BQM", "AT", "EBA", "ING1", "THI", "TCOE"],
      "Segundo Año - Tercer Semestre": ["FIS1", "MB", "GEN", "BIOE1", "EA", "TE", "TCS"],
      "Segundo Año - Cuarto Semestre": ["FIS2", "INM", "PG", "NUT", "PAR", "DES", "EXT"],
      "Tercer Año - Quinto Semestre": ["FAR", "PE", "EPAR", "TOX", "EIR", "EPI", "TOP"],
      "Tercer Año - Sexto Semestre": ["DIIB2", "CSMP1", "EINR", "MPA1", "MEQ1", "MPD2", "TEBC", "PCOE1"],
      "Cuarto Año - Séptimo Semestre": ["ECO", "LV", "RA", "MPA2", "MEQ2", "MR1", "TESOV", "INTRT"],
      "Cuarto Año - Octavo Semestre": ["GE", "PB", "POC", "ALI", "MG", "SP", "MR2", "TER", "TEBAP", "PCORM"],
      "Quinto Año - Noveno Semestre": ["SPUB", "MIAL", "CTIC", "CTIPL", "CTIRH", "AVTEC", "PSAG", "PSAA"],
      "Quinto Año - Décimo Semestre": ["PRA", "TES"]
    };

    const nombres = {
      "BQD": "Bioquímica Descriptiva",
      "AS": "Anatomía Sistemática",
      "CHG": "Citología e Histología General",
      "BF": "Biofísica",
      "IEV": "Introducción a los Estudios Veterinarios",
      "ETNO": "Etnología",
      "HSE": "Histología Sist. y Embriología",
      "BQM": "Bioquímica Metabólica",
      "AT": "Anatomía Topográfica",
      "EBA": "Etología y Bienestar Animal",
      "ING1": "Inglés I",
      "THI": "Taller Herramientas Informáticas",
      "TCOE": "Taller Comunicación Oral y Escrita",
      "FIS1": "Fisiología I",
      "MB": "Microbiología",
      "GEN": "Genética",
      "BIOE1": "Bioestadística I",
      "EA": "Experimentación Animal",
      "TE": "Taller de Ética",
      "TCS": "Taller de Ciencias Sociales",
      "FIS2": "Fisiología II",
      "INM": "Inmunología",
      "PG": "Patología General",
      "NUT": "Nutrición",
      "PAR": "Parasitología",
      "DES": "Desarrollo Sustentable",
      "EXT": "Extensión veterinaria",
      "FAR": "Farmacología",
      "PE": "Patología Especial",
      "EPAR": "Enfermedades Parasitarias",
      "TOX": "Toxicología",
      "EIR": "Enfermedades Infecciosas de Rumiantes",
      "EPI": "Epidemiología veterinaria",
      "TOP": "Técnica Operativa",
      "DIIB2": "Diseño de Investigación y Bioestadística II",
      "CSMP1": "Clínica Semiológica y Mét. Paraclínicos I",
      "EINR": "Enf. Infecciosas No Rumiantes",
      "MPA1": "Medicina de Pequeños Animales I",
      "MEQ1": "Medicina de Equinos I",
      "MPD2": "Métodos Paraclínicos de Diagnóstico II",
      "TEBC": "Taller de Etología y Bienestar en Clínica",
      "PCOE1": "Práctica Clínica Obligatoria Pequeños y Equinos",
      "ECO": "Economía",
      "LV": "Legislación Veterinaria",
      "RA": "Reproducción Animal",
      "MPA2": "Medicina de Pequeños Animales II",
      "MEQ2": "Medicina de Equinos II",
      "MR1": "Medicina de Rumiantes I",
      "TESOV": "Taller de Epidemiología y Serv. Oficiales Vet.",
      "INTRT": "Internado Rumiantes y Teriogenología",
      "GE": "Gestión de Empresas",
      "PB": "Producción de Bovinos",
      "POC": "Producción de Ovinos y Caprinos",
      "ALI": "Alimentación",
      "MG": "Mejora Genética",
      "SP": "Sistemas Productivos",
      "MR2": "Medicina de Rumiantes II",
      "TER": "Teriogenología",
      "TEBAP": "Taller de Etología y Bienestar Animal de Producción",
      "PCORM": "Práctica Clínica Obligatoria Módulo Rumiantes",
      "SPUB": "Salud Pública",
      "MIAL": "Microbiología de los Alimentos",
      "CTIC": "Ciencia, Tecnología e Inspección de Carne",
      "CTIPL": "Ciencia, Tec. e Inocuidad Leche y Derivados",
      "CTIRH": "Ciencia, Tec. e Inocuidad Recursos Hidrobiológicos",
      "AVTEC": "Avicultura y Tec. de Productos Avícolas",
      "PSAG": "Producción y Sanidad de Animales de Granja",
      "PSAA": "Producción y Sanidad de Animales Acuáticos",
      "PRA": "Practicantado",
      "TES": "Tesis de Grado"
    };

    const correlativas = {
      "ETNO": ["IEV"],
      "HSE": ["CHG"],
      "BQM": ["BQD"],
      "AT": ["AS"],
      "FIS1": ["AT", "BQM", "HSE", "BF"],
      "MB": ["AS", "BQM", "HSE"],
      "GEN": ["BQD", "HSE"],
      "EA": ["EBA"],
      "TCS": ["IEV"]
    };

    const estado = JSON.parse(localStorage.getItem("estadoMaterias") || "{}");

    function guardarEstado() {
      localStorage.setItem("estadoMaterias", JSON.stringify(estado));
    }

    function estaHabilitada(codigo) {
      if (!(codigo in correlativas)) return true;
      return correlativas[codigo].every(req => estado[req]);
    }

    function crearCheckbox(codigo, nombre) {
      const div = document.createElement("div");
      div.className = "materia";
      const check = document.createElement("input");
      check.type = "checkbox";
      check.disabled = !estaHabilitada(codigo);
      check.checked = !!estado[codigo];
      check.addEventListener("change", () => {
        estado[codigo] = check.checked;
        guardarEstado();
        location.reload();
      });
      if (!check.disabled && check.checked) {
        div.classList.add("aprobada");
      } else if (check.disabled) {
        div.classList.add("bloqueada");
      }
      const label = document.createElement("label");
      label.textContent = `${codigo} - ${nombre}`;
      div.appendChild(check);
      div.appendChild(label);
      return div;
    }

    function generarMalla() {
      const contenedor = document.getElementById("contenedor");
      Object.entries(estructura).forEach(([semestre, materias]) => {
        const section = document.createElement("div");
        section.className = "semestre";
        const h2 = document.createElement("h2");
        h2.textContent = semestre;
        section.appendChild(h2);
        materias.forEach(codigo => {
          const nombre = nombres[codigo] || "Materia sin nombre";
          section.appendChild(crearCheckbox(codigo, nombre));
        });
        contenedor.appendChild(section);
      });
    }

    generarMalla();
  </script>
</body>
</html>
