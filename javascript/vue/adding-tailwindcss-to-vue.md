# How to Configure your Vue js app to use Tailwind CSS

I am guessing you already have created your amazing vue.js project so now lets add TailwindCss

### Installing TailwindCss

```
npm i tailwindcss --save-dev
```

or

```
yarn add tailwindcss --dev
```

#### Creating a Tailwind config file

To create a the standard boilerplate for a tailwind config file type

```bash
./node_modules/.bin/tailwind init tailwind.js
```

Or if you want to create the full config file use:

```
npx tailwind init tailwind.config.js --full
```

#### Configuring PostCSS

First create a `postcss.config.js` file in the root of project:

```
touch postcss.config.js
```

Then proceed to configure Postcss to process tailwind, Do that by adding the following code to your `postcss.config.js` file. You have have to change the `tailwind.js` to `tailwind.config.js` depending what method you used above.

```javascript
module.exports = {
    plugins: [require('tailwindcss')('tailwind.js'), require('autoprefixer')()],
};
```

#### Create a tailwind css file.

Navigate to `src/assets/css` and create a new `tailwind.css` file in your `css` folder, then add the following =>

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

#### Import Tailwind into your Vue.js app

Open your `main.js` file and add: `import '@/assets/css/tailwind.css'`

You should be done!

YAY enjoy Tailwind it's bloody awesome ❤️
