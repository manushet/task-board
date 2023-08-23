<template>
    <div class='p-10 h-[100vh] bg-teal-600 overflow-auto'>
        <h1 class='text-4xl text-white flex items-center mb-10'>
            <img 
                class='mr-3' 
                width='200'
                src='@/public/images/logo.png' 
                alt='Trello Board'/>
            Task Board
        </h1>
        <TrelloBoard/>
    </div>
</template>
