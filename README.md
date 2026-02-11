# Angular + Stencil Design System - Local Testing

Acest proiect Angular este configurat pentru a testa local componentele Stencil din `/Users/gicu/design/design-respec`.

## Configurare

Proiectul folosește protocolul `portal:` din Yarn pentru a crea link-uri simbolice către pachetele locale:

```json
"@respec/design-system": "portal:../design-respec",
"@respec/angular-design-system": "portal:../design-respec/angular-design-system"
```

Acest lucru înseamnă că orice modificare în proiectul Stencil va fi reflectată automat aici după rebuild.

## Workflow de dezvoltare

### 1. Build Stencil components cu Angular wrapper

```bash
cd /Users/gicu/design/design-respec
yarn build.angular
```

### 2. Build Angular wrapper

```bash
cd /Users/gicu/design/design-respec/angular-design-system
yarn build
```

### 3. Instalează dependențele în proiectul Angular (prima dată sau după modificări în package.json)

```bash
cd /Users/gicu/design/angular-design
yarn install
```

### 4. Pornește dev server-ul Angular

```bash
cd /Users/gicu/design/angular-design
yarn start
```

Aplicația va fi disponibilă la `http://localhost:4200`

## Când să rebuilduiești

- **Modificări în componente Stencil**: Rulează `yarn build.angular` în `/Users/gicu/design/design-respec`
- **Modificări în Angular wrapper**: Rulează `yarn build` în `/Users/gicu/design/design-respec/angular-design-system`
- **După rebuild**: Angular dev server-ul va detecta automat modificările (hot reload)

## Componente disponibile

Componentele Stencil sunt disponibile prin Angular wrapper:

- `<cor-button>` - Butoane
- `<cor-typography>` - Tipografie
- `<cor-icon>` - Iconițe
- `<cor-grid>` - Grid layout
- `<cor-separator>` - Separator
- `<cor-scrollbar>` - Scrollbar custom
- `<cor-slot>` - Slot component

## Structura proiectului

```
/Users/gicu/design/
├── design-respec/              # Proiect Stencil (sursă)
│   ├── dist/                   # Build output Stencil
│   ├── angular-design-system/  # Angular wrapper
│   │   └── dist/              # Build output Angular wrapper
│   └── ...
└── angular-design/             # Proiect Angular de test (acest folder)
    ├── src/
    │   ├── app/
    │   │   ├── app.component.ts    # Importă DesignSystemModule
    │   │   └── app.component.html  # Exemple de componente
    │   └── styles.css              # Import CSS-uri Stencil
    └── package.json                # Dependențe cu portal:
```

## Note importante

1. **Portal protocol**: Link-urile simbolice sunt create automat de Yarn când folosești `portal:`
2. **TypeScript**: Erorile TypeScript din IDE vor dispărea după primul `yarn install`
3. **Hot reload**: Angular dev server-ul va reîncărca automat după rebuild-uri Stencil
4. **Revert modificări**: Pentru a reveni la workspace protocol în `angular-design-system/package.json`:
   ```json
   "@respec/design-system": "workspace:*"
   ```

## Troubleshooting

### Erori de import
Dacă vezi erori de tipul "Cannot find module '@respec/design-system'":
1. Verifică că ai rulat `yarn build.angular` în proiectul Stencil
2. Verifică că ai rulat `yarn build` în angular-design-system
3. Rulează `yarn install` din nou în proiectul Angular

### Componente nu se afișează
1. Verifică că `DesignSystemModule` este importat în `app.component.ts`
2. Verifică că CSS-urile sunt importate în `styles.css`
3. Deschide Console-ul browser-ului pentru erori

### Modificări nu se reflectă
1. Rebuilduiește pachetele Stencil și Angular wrapper
2. Angular dev server-ul ar trebui să detecteze automat modificările
3. Dacă nu, oprește și repornește `yarn start`
