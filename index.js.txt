const express = require('express');
const fs = require('fs');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 3000;

const numeros = ['5491125127839','5491132653145','5491164499481'];

let idx = 0;
const stateFile = 'state.json';

if (fs.existsSync(stateFile)) {
  try {
    const data = fs.readFileSync(stateFile);
    const obj = JSON.parse(data);
    idx = obj.idx || 0;
  } catch {
    idx = 0;
  }
}

app.get('/numero', (req, res) => {
  idx = (idx + 1) % numeros.length;
  fs.writeFileSync(stateFile, JSON.stringify({ idx }));
  res.json({ numero: numeros[idx] });
});

app.use(express.static(path.join(__dirname, 'public')));

app.listen(PORT, () => {
  console.log(`Servidor activo en el puerto ${PORT}`);
});
