# Vue-app1-todoapp
* A simple todoapp made with vue.
* Use Local Storage in browsers.
* Darkmode theme!

## == Notes == 

### === Webpack ===
webpack webpack-cli

### === Reactivity of reference ===

If a vue data references to an object, and that referenced object changes, vue can still detect that change. This is probably because the vue data does not only have the reference code as its value. And thus even though the reference code didn't change, vue can track the change in the referenced object.

In this project, there was a special case in which Vue could not detect the change. In ```this.todos = this.db.getState().todos```, this.db.getState().todos returns an array. The array is saved as a reference by this.todo. The array does not track local storage changes since it is not controlled by Vue. And thus, even when I change the local storage, the array stays the same. When I use ```this.todos.push(...)```, the change in the array is tracked. Strangely, however, after using pushing in this.db and this.todos, the array had the items pushed into it twice.

At first I thought that ```this.db.get('todos').push(newTodo).write()``` pushed the object not only to the local storage, but also to the array. And thus, the item was added twice to the array. However on second thought, this was not possible, because if the array was changed from the first line of code, Vue would have tracked the change and changed the list by its reactivness.

Using the ```cloneDeep()``` method from lodash stops this problem from happening, but I wanted to know why this problem happened at the first place.

I am planning to ask a question about this issue.

### === Lowdb / Lodash ===
Use Lowdb(which in turn uses Lodash) to connect to the Local Storage of the client(browser).
