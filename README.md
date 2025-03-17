<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tu Patente Tu auto</title>
    <style>
        body {
            background-color: #121212;
            color: #ffffff;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            background-color: #1e1e1e;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.7);
            width: 90%;
            max-width: 500px;
            text-align: center;
        }
        h1 {
            font-size: 24px;
            color: #ffcc00;
            margin-bottom: 20px;
        }
        label, input, button, select {
            width: 100%;
            margin-top: 10px;
        }
        input, select {
            padding: 10px;
            border: none;
            border-radius: 5px;
            background-color: #333333;
            color: white;
        }
        button {
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .preview-container {
            margin: 20px 0;
            padding: 20px;
            background-color: #333333;
            border-radius: 5px;
        }
        #patente-preview {
            text-align: center;
            word-wrap: break-word;
        }
        .controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 10px;
        }
        .controls label {
            font-size: 14px;
            margin-right: 10px;
        }
        .controls input[type="range"] {
            width: 70%;
        }
        .controls span {
            font-size: 14px;
            margin-left: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Tu Patente Tu auto</h1>

        <label for="patente-input">Número de Patente:</label>
        <input type="text" id="patente-input" placeholder="Ejemplo: PT-CL-2025">

        <label for="vin-input">Código VIN (Máximo 30 caracteres):</label>
        <input type="text" id="vin-input" maxlength="30" placeholder="Ingresa el código VIN">

        <label for="patente-format">Tipo de Patente:</label>
        <select id="patente-format">
            <option value="official">Formato Oficial</option>
            <option value="text-only">Solo Números</option>
        </select>

        <label for="font-family">Tipo de Letra:</label>
        <select id="font-family">
            <option value="Arial">Arial</option>
            <option value="Verdana">Verdana</option>
            <option value="Times New Roman">Times New Roman</option>
            <option value="Courier New">Courier New</option>
            <option value="Georgia">Georgia</option>
            <option value="Tahoma">Tahoma</option>
            <option value="Comic Sans MS">Comic Sans MS</option>
            <!-- Agrega más si es necesario -->
        </select>

        <div class="controls">
            <label for="font-size">Tamaño de la Letra (mm):</label>
            <input type="range" id="font-size" min="1" max="50" value="24">
            <span id="font-size-value">24 mm</span>
        </div>

        <div class="controls">
            <label for="font-width">Ancho de la Letra:</label>
            <input type="range" id="font-width" min="1" max="100" value="50">
            <span id="font-width-value">50</span>
        </div>

        <div class="preview-container">
            <h3>Vista Previa:</h3>
            <div id="patente-preview" style="font-size: 24mm; font-weight: 50; font-family: Arial;">
                Aquí aparecerá tu patente
            </div>
        </div>

        <button id="preview-button">Vista Previa</button>
        <button id="print-button">Imprimir Patente</button>
    </div>

    <script>
        // Elementos del DOM
        const patenteInput = document.getElementById("patente-input");
        const vinInput = document.getElementById("vin-input");
        const patenteFormat = document.getElementById("patente-format");
        const fontFamilySelect = document.getElementById("font-family");
        const fontSizeInput = document.getElementById("font-size");
        const fontWidthInput = document.getElementById("font-width");
        const fontSizeValue = document.getElementById("font-size-value");
        const fontWidthValue = document.getElementById("font-width-value");
        const previewButton = document.getElementById("preview-button");
        const printButton = document.getElementById("print-button");
        const patentePreview = document.getElementById("patente-preview");

        // Actualizar tamaño de letra en tiempo real
        fontSizeInput.addEventListener("input", () => {
            fontSizeValue.textContent = `${fontSizeInput.value} mm`;
        });

        // Actualizar ancho de letra en tiempo real
        fontWidthInput.addEventListener("input", () => {
            fontWidthValue.textContent = fontWidthInput.value;
        });

        // Función para mostrar la vista previa
        previewButton.addEventListener("click", () => {
            const patente = patenteInput.value.trim();
            const vin = vinInput.value.trim();
            const format = patenteFormat.value;
            const fontSize = fontSizeInput.value;
            const fontWeight = fontWidthInput.value;
            const fontFamily = fontFamilySelect.value;

            if (patente) {
                if (format === "official") {
                    patentePreview.innerHTML = `
                        <div style="text-align: center;">
                            <div style="font-size: ${fontSize}mm; font-weight: ${fontWeight}; font-family: ${fontFamily}; color: #ffcc00;">
                                ${patente}
                            </div>
                            <div style="font-size: ${fontSize / 2}mm; color: white; font-family: ${fontFamily};">${vin}</div>
                            <div style="font-size: 12px; color: white;">CHILE</div>
                        </div>
                    `;
                } else {
                    patentePreview.innerHTML = `
                        <div style="font-size: ${fontSize}mm; font-weight: ${fontWeight}; font-family: ${fontFamily}; color: #ffcc00;">
                            ${patente}
                        </div>
                        <div style="font-size: ${fontSize / 2}mm; color: white; font-family: ${fontFamily};">${vin}</div>
                    `;
                }
            } else {
                patentePreview.textContent = "Por favor, ingresa una patente válida.";
            }
        });

        // Función para imprimir la patente
        printButton.addEventListener("click", () => {
            const printWindow = window.open("", "_blank");
            const fontSize = fontSizeInput.value;
            const fontWeight = fontWidthInput.value;
            const fontFamily = fontFamilySelect.value;

            printWindow.document.write(`
                <html>
                    <head>
                        <title>Impresión de Patente</title>
                        <style>
                            body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
                            .patente { font-size: ${fontSize}mm; font-weight: ${fontWeight}; font-family: ${fontFamily}; color: #ffcc00; }
                            .vin { font-size: ${fontSize / 2}mm; color: white; font-family: ${fontFamily}; }
                        </style>
                    </head>
                    <body>
                        <div class="patente">${patenteInput.value.trim()}</div>
                        <div class="vin">${vinInput.value.trim()}</div>
                    </body>
                </html>
            `);
            printWindow.document.close();
            printWindow.print();
        });
    </script>
</body>
</html>
