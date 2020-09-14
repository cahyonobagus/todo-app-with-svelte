<script>
	import CardList from './CardList.svelte'

	let taskCardsLocalStorage = JSON.parse(localStorage.getItem('taskCards'))
	let inProgressCardsLocalStorage = JSON.parse(localStorage.getItem('inProgressCards'))
	let doneCardsLocalStorage = JSON.parse(localStorage.getItem('doneCards'))

	let taskCards = taskCardsLocalStorage ? taskCardsLocalStorage :  []
	let inProgressCards = inProgressCardsLocalStorage ?  inProgressCardsLocalStorage: []
	let doneCards = doneCardsLocalStorage ? doneCardsLocalStorage : []

	function handleEventAddCard(event){
		let data = event.detail
		if(data.listName == 'Tasks') {
			taskCards = [...taskCards, { todo: data.todo }]
			localStorage.setItem('taskCards', JSON.stringify(taskCards))
		} else if(data.listName == 'In Progress'){
			inProgressCards = [...inProgressCards, { todo: data.todo }]
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
		} else {
			doneCards = [...doneCards, { todo: data.todo }]
			localStorage.setItem('doneCards', JSON.stringify(doneCards))
		}
	}

	function handleEventDeleteCard(event) {
		let data = event.detail
		if(data.listName == 'Tasks') {
			taskCards.splice(data.index, 1)
			taskCards = taskCards
			localStorage.setItem('taskCards', JSON.stringify(taskCards))
		} else if(data.listName == 'In Progress'){
			inProgressCards.splice(data.index, 1)
			inProgressCards = inProgressCards
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
		} else {
			doneCards.splice(data.index, 1)
			doneCards = doneCards
			localStorage.setItem('doneCards', JSON.stringify(doneCards))
		}
	}

	function handleEventMoveRight(event){
		let data = event.detail
		if(data.listName == 'Tasks'){
			let cardToMove = taskCards.splice(data.index, 1)
			inProgressCards = [...inProgressCards, cardToMove[0]]
			taskCards = taskCards
			localStorage.setItem('taskCards', JSON.stringify(taskCards))
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
		} else if(data.listName == 'In Progress'){
			let cardToMove = inProgressCards.splice(data.index, 1)
			doneCards = [...doneCards, cardToMove[0]]
			inProgressCards = inProgressCards
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
			localStorage.setItem('doneCards', JSON.stringify(doneCards))
		}
	}

	function handleEventMoveLeft(event){
		let data = event.detail
		if(data.listName == 'In Progress'){
			let cardToMove = inProgressCards.splice(data.index, 1)
			taskCards = [...taskCards, cardToMove[0]]
			inProgressCards = inProgressCards
			localStorage.setItem('taskCards', JSON.stringify(taskCards))
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
		} else {
			let cardToMove = doneCards.splice(data.index, 1)
			inProgressCards = [...inProgressCards, cardToMove[0]]
			doneCards = doneCards
			localStorage.setItem('inProgressCards', JSON.stringify(inProgressCards))
			localStorage.setItem('doneCards', JSON.stringify(doneCards))
		}
	}

</script>

<div class="container is-fluid">
	<h1 class="is-size-3">Todo App</h1>
	<div class="columns">
		<CardList 
			cards={taskCards} 
			listName={'Tasks'} 
			on:addCard={handleEventAddCard} 
			on:deleteCard={handleEventDeleteCard} 
			on:moveRight={handleEventMoveRight}	
		>
		</CardList>
		<CardList 
			cards={inProgressCards} 
			listName={'In Progress'} 
			on:addCard={handleEventAddCard} 
			on:deleteCard={handleEventDeleteCard} 
			on:moveRight={handleEventMoveRight}	
			on:moveLeft={handleEventMoveLeft}
			>
		</CardList>
		<CardList 
			cards={doneCards} 
			listName={'Done'} 
			on:addCard={handleEventAddCard}
			on:deleteCard={handleEventDeleteCard}
			on:moveLeft={handleEventMoveLeft}
			>
		</CardList>
	</div>
</div>

<svelte:head>
	<link rel="stylesheet" href="/bulma.min.css" />
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" integrity="sha384-wvfXpqpZZVQGK6TAh5PVlGOfQNHSoD2xbE+QkPxCAFlNEevoEH3Sl0sibVcOQVnN" crossorigin="anonymous">
</svelte:head>