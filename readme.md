## Kubernetes and Localfed commands
## Setup Kubernetes and Docker

1. Update the package list with the command:

```
sudo apt-get update
```
2. Next, install Docker with the command:
```
sudo apt-get install docker.io
```
3. Set Docker to launch at boot by entering the following:
```
sudo systemctl enable docker
```
```
sudo systemctl status docker
```
```
sudo systemctl start docker
```
Compile scripts and styles:

```
npm start
```

## Testing

###### Chrome

1. Navigate to `chrome://extensions`

2. Click on `Load unpacked extension...`

3. Select the `dist` folder

###### Firefox

1. Navigate to `about:debugging`

2. Click on `Load temporary Add-on`

3. Select the `manifest.json` inside the `dist` folder

###### Opera

1. Navigate to `extensions`

2. Click on `Developer Mode`

3. Click on `Load unpacked extension...`

4. Select the `dist` folder

## License

[MIT License](http://zenorocha.mit-license.org/) © Zeno Rocha
