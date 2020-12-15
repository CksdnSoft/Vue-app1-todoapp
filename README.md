# Vue-app1-todoapp
todoapp made with vue.

# Webpack
webpack webpack-cli

# Notes

### Reactivity of reference
If a vue data references to a object, and that referenced object changes, vue cannot detect that change. This is because the vue data only has the reference code as its value. And since the reference code didn't change, the value of the data stays unchanged, in the eyes of vue. The only way Vue can detect changes among references is when you change the object to reference.