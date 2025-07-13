<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Malla Curricular Interactiva - Veterinaria</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f4f4f4; padding: 2rem; }
    h1 { text-align: center; }
    h2 { background-color: #005a87; color: white; padding: 0.5rem; border-radius: 5px; }
    h3 { margin-top: 0.5rem; }
    .materia {
      padding: 0.3rem 0.5rem;
      margin: 0.2rem 0;
      background: white;
      border-radius: 5px;
      display: flex;
      align-items: center;
      border-left: 5px solid transparent;
    }
    .materia input { margin-right: 10px; }
    .bloqueada { color: gray; border-left-color: black; background-color: gray }
    .aprobada { text-decoration: line-through; color: green; border-left-color: green; }
  </style>
</head>
<body>
  <h1>Malla Curricular Interactiva</h1>
  <div id="contenedor"></div>

  <script>
    const estructura = {
      "Primer Año": {
        "Semestre 1": ["BQD", "AS", "CHG", "BF", "IEV"],
        "Semestre 2": ["ETNO", "HSE", "BQM", "AT", "EBA", "ING1", "THI", "TCOE"]
      },
      "Segundo Año": {
        "Semestre 3": ["FIS1", "MB", "GEN", "BIOE1", "EA", "TE", "TCS"],
        "Semestre 4": ["FIS2", "INM", "PG", "NUT", "PAR", "DES", "EXT"]
      },
      "Tercer Año": {
        "Semestre 5": ["FAR", "PE", "EPAR", "TOX", "EIR", "EPI", "TOP"],
        "Semestre 6": ["DIIB2", "CSMP1", "EINR", "MPA1", "MEQ1", "MPD2", "TEBC", "PCOE1"]
      },
      "Cuarto Año": {
        "Semestre 7": ["ECO", "LV", "RA", "MPA2", "MEQ2", "MR1", "TESOV", "INTRT"],
        "Semestre 8": ["GE", "PB", "POC", "ALI", "MG", "SP", "MR2", "TER", "TEBAP", "PCORM"]
      },
      "Quinto Año": {
        "Semestre 9": ["SPUB", "MIAL", "CTIC", "CTIPL", "CTIRH", "AVTEC", "PSAG", "PSAA"],
        "Semestre 10": ["PRA", "TES"]
      }
    };

    const nombres = {
      BQD: "Bioquímica Descriptiva", AS: "Anatomía Sistemática", CHG: "Citología e Histología General",
      BF: "Biofísica", IEV: "Introducción a los Estudios Veterinarios", ETNO: "Etnología",
      HSE: "Histología Sistemática y Embriología", BQM: "Bioquímica Metabólica", AT: "Anatomía Topográfica",
      EBA: "Etología y Bienestar Animal", ING1: "Inglés I", THI: "Taller Herramientas Informáticas",
      TCOE: "Taller Comunicación Oral y Escrita", FIS1: "Fisiología I", MB: "Microbiología",
      GEN: "Genética", BIOE1: "Bioestadística I", EA: "Experimentación Animal", TE: "Taller de Ética",
      TCS: "Taller de Ciencias Sociales", FIS2: "Fisiología II", INM: "Inmunología", PG: "Patología General",
      NUT: "Nutrición", PAR: "Parasitología", DES: "Desarrollo Sustentable", EXT: "Extensión Veterinaria",
      FAR: "Farmacología", PE: "Patología Especial", EPAR: "Enfermedades Parasitarias", TOX: "Toxicología",
      EIR: "Enf. Infecciosas Rumiantes", EPI: "Epidemiología veterinaria", TOP: "Técnica Operativa",
      DIIB2: "Diseño de Investigación y Bioestadística II", CSMP1: "Clínica Semiológica y Mét. Paraclínicos I",
      EINR: "Enf. Infecciosas No Rumiantes", MPA1: "Medicina Pequeños Animales I", MEQ1: "Medicina Equinos I",
      MPD2: "Métodos Paraclínicos Diagnóstico II", TEBC: "Taller de Etología y Bienestar Clínica", PCOE1: "Práctica Clínica Peq. y Equinos",
      ECO: "Economía", LV: "Legislación Veterinaria", RA: "Reproducción Animal", MPA2: "Medicina Pequeños Animales II",
      MEQ2: "Medicina Equinos II", MR1: "Medicina Rumiantes I", TESOV: "Taller Epidem. y Servicios Oficiales",
      INTRT: "Internado Rumiantes y Teriogenología", GE: "Gestión de Empresas", PB: "Producción de Bovinos",
      POC: "Producción de Ovinos y Caprinos", ALI: "Alimentación", MG: "Mejora Genética", SP: "Sistemas Productivos",
      MR2: "Medicina Rumiantes II", TER: "Teriogenología", TEBAP: "Taller de Etología y Bienestar Producción",
      PCORM: "Práctica Clínica Rumiantes", SPUB: "Salud Pública", MIAL: "Microbiología Alimentos",
      CTIC: "Ciencia, Tecnología e Inspección Carne", CTIPL: "Ciencia e Inocuidad Leche", CTIRH: "Ciencia e Inocuidad Recursos Hidrob.",
      AVTEC: "Avicultura y Tec. Productos Avícolas", PSAG: "Producción y Sanidad Animales de Granja",
      PSAA: "Producción y Sanidad Animales Acuáticos", PRA: "Practicantado", TES: "Tesis de Grado"
    };

    const correlativas = {
      ETNO: ["IEV"], HSE: ["CHG"], BQM: ["BQD"], AT: ["AS"], 
      FIS1: ["AT", "BQM", "HSE", "BF"], MB: ["AS", "BQM", "HSE"], 
      GEN: ["BQD", "HSE"], EA: ["EBA"], TCS: ["IEV"],
      FAR: ["FIS2", "INM"], PE: ["FIS2", "PG"], EPAR: ["FIS2", "PAR"],
      TOX: ["FIS2", "PG"], EIR: ["FIS2", "INM", "PG", "MB"],
      EPI: ["BIOE1", "MB"], TOP: ["FIS1", "PG"], 
      DIIB2: ["EBA", "BIOE1", "EA"], CSMP1: ["FIS2"],
      EINR: ["FIS2", "INM", "PG", "MB"], MPA1: ["FAR", "PG", "PAR", "TOX"],
      MEQ1: ["FAR", "PG", "PAR", "TOX"], MPD2: ["FIS2", "PG"],
      TEBC: ["EBA"], PCOE1: ["FAR", "PG", "PAR", "TOX"],
      ECO: ["DES"], LV: ["EPAR", "EINR", "EIR"],
      RA: ["FIS2"], MPA2: ["MPA1", "PE", "EPAR", "TOP"],
      MEQ2: ["MEQ1", "PE", "EPAR", "TOP"], MR1: ["FAR", "PG", "PAR", "TOX", "EIR"],
      TESOV: ["EPI", "EPAR", "EINR", "EIR"], INTRT: ["FAR", "PG", "PAR", "TOX", "EIR"],
      GE: ["ECO"], PB: ["MR1", "PE", "EPAR", "TOP", "RA"],
      POC: ["MR1", "PE", "EPAR", "TOP", "RA"], ALI: ["NUT", "FIS2"],
      MG: ["FIS2", "GEN", "DIIB2"], SP: ["ECO"],
      MR2: ["MR1", "PE", "EPAR", "TOP", "RA"], TER: ["RA"],
      TEBAP: ["EBA", "DES", "FIS2"], PCORM: ["MR1", "PE", "EPAR", "TOP", "RA"],
      SPUB: ["EPI", "EPAR", "EINR", "EIR", "LV"],
      MIAL: ["EPI", "LV"], CTIC: ["MR1", "PE", "EPAR", "EINR", "PB", "POC"],
      CTIPL: ["MR1", "PE", "PB"], CTIRH: ["FAR", "PG", "TOX", "EPAR"],
      AVTEC: ["FAR", "PAR", "TOX", "EINR"], PSAG: ["FAR", "PG", "PAR", "TOX", "NUT"],
      PSAA: ["PG", "PAR", "NUT"]
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
      Object.entries(estructura).forEach(([anio, semestres]) => {
        const bloque = document.createElement("div");
        const titulo = document.createElement("h2");
        titulo.textContent = anio;
        bloque.appendChild(titulo);

        const fila = document.createElement("div");
        fila.style.display = "flex";
        fila.style.flexWrap = "wrap";
        fila.style.gap = "2rem";

        Object.entries(semestres).forEach(([semestre, materias]) => {
          const columna = document.createElement("div");
          columna.style.flex = "1";
          const subtitulo = document.createElement("h3");
          subtitulo.textContent = semestre;
          columna.appendChild(subtitulo);
          materias.forEach(codigo => {
            const nombre = nombres[codigo] || "Materia sin nombre";
            columna.appendChild(crearCheckbox(codigo, nombre));
          });
          fila.appendChild(columna);
        });

        bloque.appendChild(fila);
        contenedor.appendChild(bloque);
      });
    }

    generarMalla();
  </script>
</body>
</html>
