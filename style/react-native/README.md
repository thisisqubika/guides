# React Native Styleguide

This guide reflects the styles and conventions we use for our React Native applications. 

This is an attempt to encourage patterns that accomplish the following goals (in
rough priority order):

 1. Increased rigor, and decreased likelihood of programmer error
 1. Increased clarity of intent
 1. Reduced verbosity
 1. Fewer debates about aesthetics

If you have suggestions, please see our [contribution guidelines](/#contributing).

## Based on

We didn't want to reinvent the wheel, so we follow the [Airbnb Guidelines](https://github.com/airbnb/javascript). This are a fully-featured set of styles and recomendations that have been widely adopted by the community. For further information, please refer to it's repository.

Since React Native is based in React and it relies on JSX for its UI definition, we also use the [React/JSX Guidelines](https://github.com/airbnb/javascript/tree/master/react). This were also created by Airbnb. 

Even though this guidelines are widely adopted by the community, we do have some exceptions to this rules in order to better fit the team needs. This are explained in the following section or can be found in the [linter configuration file](https://github.com/moove-it/react-native-template/blob/master/.eslintrc.json) in the React Native Template.

## Exceptions

* **[Forbidden prop types](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md)**: We do not follow this rule because it's too intrusive with which propTypes are correct and which are too generic.
* **[JSX filename extension](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)**: We are not keen on forcing developer to use .jsx files since it provides no real benefit.
* **[Import order](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/order.md)**: This rule is not actually disencouraged, but actually configured to better adapt to what the team prefers. The default order for the imports is `"builtin", "external", "parent", "sibling", "index"`. We do prefer having the following order: `"builtin", "external", "parent", "sibling", "index"`

## Tools

The tool used to enforce this is [ESLint](https://eslint.org/), which is almost the de-facto tool for enforcing and checking in Javascript. 

We highly recommend using the ESlint integrations with the IDE or code editor that the developer is using. The following are the most popular ones:

* Visual Studio Code [Link](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* Atom [Link](https://atom.io/packages/linter-eslint)
* Sublime [Link](https://packagecontrol.io/packages/ESLint)
* WebStorm [Link](https://www.jetbrains.com/help/webstorm/eslint.html)