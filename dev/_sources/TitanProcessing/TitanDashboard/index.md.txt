# Titan-Dashboard

Titan-Dashboard is the dashboard that we use for 

## Running

Make sure you have npm and nodejs installed on your system. (You can also use pnpm if you like.) Then:

```bash
git clone https://github.com/titan2022/Titan-Dashboard
cd Titan-Dashboard
npm install
npm run start
```

### Installing npm and nodejs

On openSUSE and openSUSE derivatives:
```bash
sudo zypper in nodejs-default npm-default pnpm
```

## Usage

Only the field view in the top right corner currently works. All the other panels (CPU/RAM usage, time, etc) don't work.

### Generating field images

Underneath the field view is a button to generate field images. Click that button, then click "Capture", in order to generate field images. In order to change which field image to generate, you need to modify `src/gen/ui/index.js`.
