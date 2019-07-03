# package.json

"scripts": {
  "start": "webpack-dev-server --mode development",
  "build": "webpack --mode production"
}

## watch
npm run build -- --watch

# webpack.config.js

## WDS

devServer: {
  stats: "errors-only",
  host: process.env.HOST,
  port: process.env.PORT,
  open: true,
  overlay: true    # to capture complation warnings and errors
}

dotenv: define environment variables through a .env file.

> Note: Enable devServer.historyApiFallback if you are using HTML5 History API based routing.
> Note: For better output, consider **error-overlay-webpack-plugin**

## Style

{
  rules: [
    {
      test: /\.css$/,
      include: "",
      exclude: "",
      use: ["style-loader", "css-loader"], // equal to styleLoader(cssLoader(input))
    },
  ],
}

### with LESS
use: ["style-loader", "css-loader", "less-loader"]


### loader with option
{
  use: {
    loader: "babel-loader",
    options: {
      presets: ["env"],
    },
  },
}
or more than one loader
{
  use: [
    {
      loader: "babel-loader",
      options: {
        presets: ["env"],
      },
    },
  ]
}




