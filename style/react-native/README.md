# React Native Styleguide

This guide reflects the styles and conventions we use for our React Native applications. 

This is an attempt to encourage patterns that accomplish the following goals (in
rough priority order):

 1. Increased rigor, and decreased likelihood of programmer error
 2. Increased clarity of intent
 3. Reduced verbosity
 4. Fewer debates about aesthetics

If you have suggestions, please see our [contribution guidelines](/#contributing).

## Based on

Since React Native uses Javascript and React/JSX, we follow both this guidelines for development. Please refer to both of them as a styleguide for React Native development [here](/style/ecmascript-6+#ecmascript-6+-style-guide) and [here](/style/react-jsx).

## Linter

The tool used to enforce this is [ESLint](https://eslint.org/), which is almost the de-facto tool for enforcing and checking in Javascript. We use the airbnb ESLint package to enforce the previously mentioned guides. 

### Exceptions

In order to better fit the team needs and the differences between our guides and Airbnb's guides, the following exceptions were defined. They can also be found in the [linter configuration file](https://github.com/moove-it/react-native-template/blob/master/.eslintrc.json) in the React Native Template.

* **[Forbidden prop types](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md)**: We do not follow this rule because it's too intrusive with which propTypes are correct and which are too generic.
* **[JSX filename extension](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)**: We are not keen on forcing developer to use .jsx files since it provides no real benefit.
* **[Import order](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/order.md)**: This rule is not actually disencouraged, but actually configured to better adapt to what the team prefers. The default order for the imports is `"builtin", "external", "parent", "sibling", "index"`. We do prefer having the following order: `"builtin", "external", "parent", "sibling", "index"`

### React Native Specific design decisiones

#### Platform specific code

Always prefer having a `.android.js` and a `.ios.js` file instead of having platform checks `if Platform.OS === 'ios' ...`. The resulting code will be clearer and more extensible, since adding new changes for each platform will be easier. 

### Tools

We highly recommend using the ESlint integrations with the IDE or code editor that the developer is using. The following are the most popular ones:

* Visual Studio Code [Link](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* Atom [Link](https://atom.io/packages/linter-eslint)
* Sublime [Link](https://packagecontrol.io/packages/ESLint)
* WebStorm [Link](https://www.jetbrains.com/help/webstorm/eslint.html)