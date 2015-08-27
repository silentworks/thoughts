In many popular frameworks these days you can lazy load your actions in order to not invoke every action unless it is the one matched that should be dispatched. 

In Laravel you can use the `@` symbol to state your action for the defined route, while in Slim you would use the `:` symbol. The issue I am finding with this approach is that my IDE cannot go to the defining class by ctrl clicking on the string, nor can I refactor a class easily because my route actions are all defined as strings.
