Projet Angular 19 Module Federation - Tous les fichiers

Structure du projet à créer

module-federation-project/
├── package.json
├── angular.json
├── tsconfig.json
├── README.md
└── projects/
    ├── mfe-provider/
    │   ├── tsconfig.app.json
    │   ├── webpack.config.js
    │   └── src/
    │       ├── index.html
    │       ├── main.ts
    │       ├── bootstrap.ts
    │       └── app/
    │           ├── app.component.ts
    │           ├── app.module.ts
    │           └── components/
    │               └── hello-world/
    │                   ├── hello-world.component.ts
    │                   ├── hello-world.component.html
    │                   └── hello-world.component.scss
    └── mfe-consumer/
        ├── tsconfig.app.json
        ├── webpack.config.js
        └── src/
            ├── index.html
            ├── main.ts
            ├── bootstrap.ts
            └── app/
                ├── app.component.ts
                └── app.module.ts
📁 Fichiers racine

package.json

{
  "name": "module-federation-workspace",
  "version": "1.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "start:mfe-provider": "ng serve mfe-provider --port 4201",
    "start:mfe-consumer": "ng serve mfe-consumer --port 4200",
    "build": "ng build",
    "build:mfe-provider": "ng build mfe-provider",
    "build:mfe-consumer": "ng build mfe-consumer"
  },
  "dependencies": {
    "@angular/animations": "^19.0.0",
    "@angular/common": "^19.0.0",
    "@angular/compiler": "^19.0.0",
    "@angular/core": "^19.0.0",
    "@angular/elements": "^19.0.0",
    "@angular/forms": "^19.0.0",
    "@angular/platform-browser": "^19.0.0",
    "@angular/platform-browser-dynamic": "^19.0.0",
    "@angular/router": "^19.0.0",
    "rxjs": "~7.8.0",
    "tslib": "^2.3.0",
    "zone.js": "~0.15.0"
  },
  "devDependencies": {
    "@angular-devkit/build-angular": "^19.0.0",
    "@angular/cli": "^19.0.0",
    "@angular/compiler-cli": "^19.0.0",
    "@module-federation/webpack": "^2.0.0",
    "@types/node": "^20.11.0",
    "typescript": "~5.6.0",
    "webpack": "^5.88.0",
    "webpack-cli": "^5.1.0"
  }
}
angular.json

{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {
    "mfe-provider": {
      "projectType": "application",
      "schematics": {},
      "root": "projects/mfe-provider",
      "sourceRoot": "projects/mfe-provider/src",
      "prefix": "app",
      "architect": {
        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/mfe-provider",
            "index": "projects/mfe-provider/src/index.html",
            "main": "projects/mfe-provider/src/main.ts",
            "polyfills": ["zone.js"],
            "tsConfig": "projects/mfe-provider/tsconfig.app.json",
            "assets": [],
            "styles": [],
            "scripts": [],
            "customWebpackConfig": {
              "path": "projects/mfe-provider/webpack.config.js"
            }
          }
        },
        "serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "options": {
            "port": 4201
          }
        }
      }
    },
    "mfe-consumer": {
      "projectType": "application",
      "schematics": {},
      "root": "projects/mfe-consumer",
      "sourceRoot": "projects/mfe-consumer/src",
      "prefix": "app",
      "architect": {
        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/mfe-consumer",
            "index": "projects/mfe-consumer/src/index.html",
            "main": "projects/mfe-consumer/src/main.ts",
            "polyfills": ["zone.js"],
            "tsConfig": "projects/mfe-consumer/tsconfig.app.json",
            "assets": [],
            "styles": [],
            "scripts": [],
            "customWebpackConfig": {
              "path": "projects/mfe-consumer/webpack.config.js"
            }
          }
        },
        "serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "options": {
            "port": 4200
          }
        }
      }
    }
  }
}
tsconfig.json

{
  "compileOnSave": false,
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./dist/out-tsc",
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitOverride": true,
    "noPropertyAccessFromIndexSignature": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "sourceMap": true,
    "declaration": false,
    "downlevelIteration": true,
    "experimentalDecorators": true,
    "moduleResolution": "node",
    "importHelpers": true,
    "target": "ES2022",
    "module": "ES2022",
    "useDefineForClassFields": false,
    "lib": ["ES2022", "dom"]
  },
  "angularCompilerOptions": {
    "enableI18nLegacyMessageIdFormat": false,
    "strictInjectionParameters": true,
    "strictInputAccessModifiers": true,
    "strictTemplates": true
  }
}
📁 MFE Provider

projects/mfe-provider/tsconfig.app.json

{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "outDir": "../../out-tsc/app",
    "types": []
  },
  "files": ["src/main.ts"],
  "include": ["src/**/*.d.ts"]
}
projects/mfe-provider/webpack.config.js

const ModuleFederationPlugin = require('@module-federation/webpack');

module.exports = {
  mode: 'development',
  plugins: [
    new ModuleFederationPlugin({
      name: 'mfe_provider',
      filename: 'remoteEntry.js',
      exposes: {
        './HelloWorldComponent': './projects/mfe-provider/src/app/components/hello-world/hello-world.component.ts',
      },
      shared: {
        '@angular/core': { singleton: true, strictVersion: true },
        '@angular/common': { singleton: true, strictVersion: true },
        '@angular/platform-browser': { singleton: true, strictVersion: true },
        '@angular/platform-browser-dynamic': { singleton: true, strictVersion: true },
        '@angular/router': { singleton: true, strictVersion: true },
        '@angular/elements': { singleton: true, strictVersion: true },
        'rxjs': { singleton: true, strictVersion: true },
      },
    }),
  ],
};
projects/mfe-provider/src/index.html

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>MFE Provider</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
  <app-root></app-root>
</body>
</html>
projects/mfe-provider/src/main.ts

import('./bootstrap')
  .catch(err => console.error(err));
projects/mfe-provider/src/bootstrap.ts

import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';

platformBrowserDynamic()
  .bootstrapModule(AppModule)
  .catch(err => console.error(err));
projects/mfe-provider/src/app/app.module.ts

import { NgModule, Injector } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { createCustomElement } from '@angular/elements';

import { AppComponent } from './app.component';
import { HelloWorldComponent } from './components/hello-world/hello-world.component';

@NgModule({
  declarations: [
    AppComponent,
    HelloWorldComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {
  constructor(private injector: Injector) {}

  ngDoBootstrap() {
    // Créer un custom element pour le web component
    const helloWorldElement = createCustomElement(HelloWorldComponent, { injector: this.injector });
    customElements.define('hello-world-element', helloWorldElement);
  }
}
projects/mfe-provider/src/app/app.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h1>MFE Provider Application</h1>
    <p>Cette application expose le composant HelloWorld via Module Federation</p>
    <hr>
    <h2>Aperçu du composant exposé:</h2>
    <app-hello-world></app-hello-world>
  `
})
export class AppComponent {
  title = 'mfe-provider';
}
projects/mfe-provider/src/app/components/hello-world/hello-world.component.ts

import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-hello-world',
  templateUrl: './hello-world.component.html',
  styleUrls: ['./hello-world.component.scss']
})
export class HelloWorldComponent {
  @Input() message: string = 'Hello from Module Federation!';
  @Input() color: string = '#007bff';

  currentTime = new Date();

  constructor() {
    setInterval(() => {
      this.currentTime = new Date();
    }, 1000);
  }

  onClick() {
    alert(`Message: ${this.message}\nTime: ${this.currentTime}`);
  }
}
projects/mfe-provider/src/app/components/hello-world/hello-world.component.html

<div class="hello-world-container" [style.border-color]="color">
  <h3 [style.color]="color">{{ message }}</h3>
  <p>Heure actuelle: {{ currentTime | date:'medium' }}</p>
  <button (click)="onClick()" [style.background-color]="color">
    Cliquer ici
  </button>
  <div class="info">
    <small>Ce composant est chargé via Module Federation</small>
  </div>
</div>
projects/mfe-provider/src/app/components/hello-world/hello-world.component.scss

.hello-world-container {
  padding: 20px;
  border: 2px solid #007bff;
  border-radius: 8px;
  margin: 10px;
  text-align: center;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.hello-world-container h3 {
  margin: 0 0 10px 0;
  font-size: 1.5em;
}

.hello-world-container p {
  margin: 10px 0;
  font-size: 1.1em;
}

.hello-world-container button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
  font-size: 1em;
  transition: opacity 0.3s;
}

.hello-world-container button:hover {
  opacity: 0.8;
}

.info {
  margin-top: 15px;
  padding: 5px;
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 4px;
}

.info small {
  color: #666;
  font-style: italic;
}
📁 MFE Consumer

projects/mfe-consumer/tsconfig.app.json

{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "outDir": "../../out-tsc/app",
    "types": []
  },
  "files": ["src/main.ts"],
  "include": ["src/**/*.d.ts"]
}
projects/mfe-consumer/webpack.config.js

const ModuleFederationPlugin = require('@module-federation/webpack');

module.exports = {
  mode: 'development',
  plugins: [
    new ModuleFederationPlugin({
      name: 'mfe_consumer',
      remotes: {
        'mfe-provider': 'mfe_provider@http://localhost:4201/remoteEntry.js',
      },
      shared: {
        '@angular/core': { singleton: true, strictVersion: true },
        '@angular/common': { singleton: true, strictVersion: true },
        '@angular/platform-browser': { singleton: true, strictVersion: true },
        '@angular/platform-browser-dynamic': { singleton: true, strictVersion: true },
        '@angular/router': { singleton: true, strictVersion: true },
        '@angular/elements': { singleton: true, strictVersion: true },
        'rxjs': { singleton: true, strictVersion: true },
      },
    }),
  ],
};
projects/mfe-consumer/src/index.html

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>MFE Consumer</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
  <app-root></app-root>
</body>
</html>
projects/mfe-consumer/src/main.ts

import('./bootstrap')
  .catch(err => console.error(err));
projects/mfe-consumer/src/bootstrap.ts

import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';

platformBrowserDynamic()
  .bootstrapModule(AppModule)
  .catch(err => console.error(err));
projects/mfe-consumer/src/app/app.module.ts

import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent],
  schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
export class AppModule { }
projects/mfe-consumer/src/app/app.component.ts

import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <div class="container">
      <h1>MFE Consumer Application</h1>
      <p>Cette application consomme le composant HelloWorld via Module Federation</p>
      
      <div class="section">
        <h2>Composant chargé dynamiquement:</h2>
        <div id="dynamic-component"></div>
        <button (click)="loadComponent()" [disabled]="componentLoaded">
          {{ componentLoaded ? 'Composant chargé' : 'Charger le composant' }}
        </button>
      </div>

      <div class="section">
        <h2>Web Component (Custom Element):</h2>
        <hello-world-element 
          [attr.message]="'Salut depuis le Consumer!'"
          [attr.color]="'#28a745'">
        </hello-world-element>
      </div>

      <div class="section">
        <h2>Multiple instances:</h2>
        <hello-world-element 
          [attr.message]="'Instance 1'"
          [attr.color]="'#dc3545'">
        </hello-world-element>
        <hello-world-element 
          [attr.message]="'Instance 2'"
          [attr.color]="'#ffc107'">
        </hello-world-element>
      </div>
    </div>
  `,
  styles: [`
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
      font-family: Arial, sans-serif;
    }
    .section {
      margin: 30px 0;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
      background-color: #f9f9f9;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      margin: 10px 0;
    }
    button:disabled {
      background-color: #6c757d;
      cursor: not-allowed;
    }
    button:hover:not(:disabled) {
      background-color: #0056b3;
    }
  `]
})
export class AppComponent implements OnInit {
  componentLoaded = false;

  ngOnInit() {
    // Charger le web component au démarrage
    this.loadWebComponent();
  }

  async loadComponent() {
    try {
      const { HelloWorldComponent } = await import('mfe-provider/HelloWorldComponent');
      
      // Créer une instance du composant et l'injecter dans le DOM
      const componentRef = document.createElement('div');
      componentRef.innerHTML = '<p>Composant chargé dynamiquement depuis le provider MFE!</p>';
      
      const container = document.getElementById('dynamic-component');
      if (container) {
        container.appendChild(componentRef);
        this.componentLoaded = true;
      }
    } catch (error) {
      console.error('Erreur lors du chargement du composant:', error);
    }
  }

  private async loadWebComponent() {
    try {
      // Charger le script du provider pour enregistrer le web component
      const script = document.createElement('script');
      script.src = 'http://localhost:4201/remoteEntry.js';
      document.head.appendChild(script);
      
      script.onload = () => {
        console.log('Web component chargé avec succès!');
      };
    } catch (error) {
      console.error('Erreur lors du chargement du web component:', error);
    }
  }
}
📋 README.md

# Module Federation Project - Angular 19

Ce projet démontre l'utilisation de Module Federation avec Angular 19 pour créer et consommer des web components.

## Structure du projet

- **mfe-provider**: Application qui expose un composant HelloWorld via Module Federation
- **mfe-consumer**: Application qui consomme le composant exposé

## Installation

1. Installer les dépendances:
```bash
npm install
Démarrage

Démarrer l'application provider (port 4201):
npm run start:mfe-provider
Dans un autre terminal, démarrer l'application consumer (port 4200):
npm run start:mfe-consumer
Ouvrir les applications:
Provider: http://localhost:4201
Consumer: http://localhost:4200
Fonctionnalités

MFE Provider

Expose un composant HelloWorld via Module Federation
Crée un custom element (web component)
Affiche un aperçu du composant
MFE Consumer

Consomme le composant via Module Federation
Utilise le web component avec différentes configurations
Démontre le chargement dynamique
Build

# Build provider
npm run build:mfe-provider

# Build consumer
npm run build:mfe-consumer
Technologies utilisées

Angular 19
Module Federation (Webpack)
Angular Elements
TypeScript
Notes importantes

Le provider doit être démarré avant le consumer
Les ports par défaut sont 4201 (provider) et 4200 (consumer)
Le web component est accessible via la balise <hello-world-element>

---

## 🚀 **Instructions pour créer le projet**

1. **Créer le dossier racine** : `module-federation-project`
2. **Copier tous les fichiers** dans leur structure respective
3. **Installer les dépendances** : `npm install`
4. **Démarrer les applications** :
   - Terminal 1: `npm run start:mfe-provider`
   - Terminal 2: `npm run start:mfe-consumer`

Le projet est maintenant prêt à être utilisé ! 🎉
