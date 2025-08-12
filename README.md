<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Sube tu video para transcripción</title>
  <style>
    /* Tipografía moderna */
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

    * {
      box-sizing: border-box;
    }

    body {
      font-family: 'Inter', sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: #fff;
      margin: 0;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 20px;
    }

    .container {
      background: rgba(255, 255, 255, 0.1);
      padding: 40px 30px;
      border-radius: 12px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.2);
      max-width: 400px;
      width: 100%;
      text-align: center;
      backdrop-filter: blur(10px);
    }

    h1 {
      margin-bottom: 25px;
      font-weight: 600;
      font-size: 1.8rem;
      letter-spacing: 0.04em;
      text-shadow: 0 2px 4px rgba(0,0,0,0.3);
    }

    label {
      display: block;
      margin-bottom: 8px;
      font-weight: 600;
      font-size: 1rem;
      text-align: left;
      color: #f0e9ff;
      text-shadow: 0 1px 2px rgba(0,0,0,0.3);
    }

    input[type="text"] {
      width: 100%;
      padding: 12px 15px;
      border-radius: 8px;
      border: none;
      font-size: 1rem;
      font-weight: 400;
      outline: none;
      transition: box-shadow 0.3s ease;
      background: rgba(255,255,255,0.15);
      color: #fff;
      box-shadow: inset 0 0 5px rgba(255,255,255,0.3);
    }

    input[type="text"]::placeholder {
      color: #d6c8ffcc;
      font-style: italic;
    }

    input[type="text"]:focus {
      box-shadow: 0 0 10px 2px #a18aff;
      background: rgba(255,255,255,0.25);
    }

    button {
      margin-top: 25px;
      width: 100%;
      padding: 14px 0;
      font-size: 1.1rem;
      font-weight: 600;
      color: #fff;
      background: #8860ff;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      box-shadow: 0 4px 12px rgba(136, 96, 255, 0.7);
      transition: background 0.3s ease, box-shadow 0.3s ease;
      letter-spacing: 0.05em;
    }

    button:hover {
      background: #6c43d9;
      box-shadow: 0 6px 18px rgba(108, 67, 217, 0.8);
    }

    /* Mensajes de alerta */
    #responseMessage {
      margin-top: 20px;
      font-weight: 600;
      font-size: 1rem;
      min-height: 1.2em;
      color: #ffdd57;
      text-shadow: 0 1px 2px rgba(0,0,0,0.3);
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Sube tu video para transcripción</h1>
    <form id="videoForm" autocomplete="off" spellcheck="false">
      <label for="videoUrl">URL del video:</label>
      <input
        type="text"
        id="videoUrl"
        name="videoUrl"
        placeholder="https://drive.google.com/..."
        required
        autofocus
      />
      <button type="submit">Enviar</button>
    </form>
    <div id="responseMessage"></div>
  </div>

  <script>
    const form = document.getElementById('videoForm');
    const responseMessage = document.getElementById('responseMessage');

    form.addEventListener('submit', async function (e) {
      e.preventDefault();

      responseMessage.textContent = 'Enviando...';

      const videoUrl = document.getElementById('videoUrl').value.trim();

      try {
        const response = await fetch('https://aecode.app.n8n.cloud/webhook/video-to-transcript', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ videoUrl }),
        });

        if (!response.ok) throw new Error(`Error HTTP: ${response.status}`);

        const data = await response.json();

        responseMessage.style.color = '#a1ff57';
        responseMessage.textContent = '✅ Video enviado correctamente.';
        form.reset();
        form.querySelector('input').focus();

        console.log('Respuesta n8n:', data);
      } catch (error) {
        responseMessage.style.color = '#ff6f6f';
        responseMessage.textContent = '❌ Error al enviar el video. Intenta de nuevo.';
        console.error('Error al enviar:', error);
      }
    });
  </script>
</body>
</html>
