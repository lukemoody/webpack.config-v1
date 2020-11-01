const webpack = require('webpack');
const merge = require('webpack-merge');
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const FriendlyErrorsWebpackPlugin = require('friendly-errors-webpack-plugin');
const BrowserSyncPlugin = require('browser-sync-webpack-plugin');

/**
 * Settings for BrowserSync
 */
const settings = {
  host: 'localhost', // The BrowserSync hostname
  port: 3000, // The port to run BrowserSync's server on
  proxy: 'xxx', // Proxy URL
};

/**
 * Configuration object.
 */
const webpackConfig = {
  /**
   * Create the entry points.
   */
  entry: {
    main: './src/js/index.js'
  },

  /**
   * Create the output files.
   */
  output: {
    /**
     * [name] allows for the entry object keys to be used as file names.
     */
    filename: '[name].js',
    /**
     * Tell the output to point back at the Webpack Dev Server. Otherwise,
     * it will point at your LAMP stack when Webpack used with PHP.
     */
    publicPath: '/',
    /**
     * Specify the path to the JS files.
     */
    path: path.resolve(__dirname, 'dist'),
    /**
     * Source map file name
     */
    // sourceMapFilename: '[file].js.map'
    sourceMapFilename: '[file].map'
  },

  /**
   * Define module rules
   */
  module: {
    rules: [
      {
        /**
         * Look for any JS files.
         */
        test: /\.js$/,
        exclude: /node_modules/,
        /**
         * Use babel loader to transpile the JS files.
         */
        use: [
          {
            loader: 'babel-loader'
          }
          // { parser: { amd: false } }
        ]
      },
      /**
       * Look for SCSS files and convert to CSS
       */
      {
        test: /\.(s*)css$/,
        include: path.resolve(__dirname, 'src/scss/main.scss'),
        use: [
          MiniCssExtractPlugin.loader, // 3. Extract css into files
          // "style-loader", // 3. Inject styles into DOM
          'css-loader', // 2. Turns css into commonjs
          'sass-loader' // 1. Turns sass into css
        ]
      },
      /**
       * Allow path to assets
       */
      {
        test: /\.(ttf|otf|eot|woff2?|png|jpe?g|gif|svg|ico)$/,
        use: ['file-loader']
      }
    ]
  },

  /**
   * Configure how performance hints are shown. An asset that is over 250kb,
   * webpack will emit a warning notifying you of this.
   * False removes warning
   */
  performance: { hints: false },

  devtool: false,

  plugins: [
    /**
     * This plugin extracts CSS into separate files. It creates a CSS file per JS file which contains CSS.
     * It supports On-Demand-Loading of CSS and SourceMaps.
     */
    new MiniCssExtractPlugin({
      filename: '[name].css',
      chunkFilename: '[id].css'
    }),
    /**
     * Minimise CSS output (currently for both development & production environments)
     */
    new OptimizeCssAssetsPlugin({
      assetNameRegExp: /\.optimize\.css$/g,
      cssProcessor: require('cssnano'),
      cssProcessorPluginOptions: {
        preset: ['default', { discardComments: { removeAll: true } }]
      },
      canPrint: true
    }),
    /**
     * Friendly-errors-webpack-plugin recognizes certain classes of webpack errors and cleans,
     * aggregates and prioritizes them to provide a better Developer Experience.
     */
    new FriendlyErrorsWebpackPlugin(),
    /**
     * BrowserSync
     */
    new BrowserSyncPlugin({
      host: settings.host,
      port: settings.port,
      proxy: settings.proxy,
      open: false
    }),
    /**
     * Source Maps
     * CSS excluded
     */
    new webpack.SourceMapDevToolPlugin({
      filename: '[file].map',
      exclude: ['main.css']
    })
  ],

  /**
   * SplitChunks
   */
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          chunks: 'async',
          priority: 1
        }
      }
    }
  }
};

// Export the config object
module.exports = webpackConfig;
