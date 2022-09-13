const todoTask=document.querySelector(".todo-task");
const taskList=document.querySelector(".task-list");
const formSubmit=document.querySelector(".btn");
const formInput=document.querySelector("#new-task");

formSubmit.addEventListener("click",addTasks);
document.addEventListener("DOMContentLoaded",localrender);
function addTasks(e)
{
    e.preventDefault();
    var taskDetail=formInput.value;
    createListContainer(taskDetail);
    saveLocalstorage(taskDetail);
    formInput.value=null;
       
}
function createListContainer(taskDetail)
{
    if(taskDetail.length>=1)
    {
        const taskDiv=document.createElement("div");
        taskDiv.classList.add("task");
        const taskListItem=document.createElement("li");
        taskListItem.classList.add("list-item");
        const confirmButton=document.createElement("button");
        confirmButton.innerHTML="<i class='fas fa-check-circle ok'></i>";
        confirmButton.classList.add("confirm-btn")
        const deleteButton=document.createElement("button");
        deleteButton.innerHTML="<i class='fas fa-trash ok'></i>";
        deleteButton.classList.add("delete-btn");
        taskListItem.innerText=taskDetail;
        taskDiv.append(taskListItem);
        taskDiv.append(confirmButton);
        taskDiv.append(deleteButton);
        taskList.append(taskDiv);
    }
}

function createListContainerforLoad(taskDetail)
{
    if(taskDetail.length>=1)
    {
        const taskDiv=document.createElement("div");
        taskDiv.classList.add("task");
        const taskListItem=document.createElement("li");
        taskListItem.classList.add("list-item");
        const confirmButton=document.createElement("button");
        confirmButton.innerHTML="<i class='fas fa-check-circle ok'></i>";
        confirmButton.classList.add("confirm-btn")
        const deleteButton=document.createElement("button");
        deleteButton.innerHTML="<i class='fas fa-trash ok'></i>";
        deleteButton.classList.add("delete-btn");
        taskListItem.innerText=taskDetail;
        var complete;
        if(localStorage.getItem("complete")=== null)
         {
         complete=[];
         } else{
          complete=JSON.parse(localStorage.getItem("complete"));
         }
         if(complete.indexOf(taskDetail)>=0)
         {
             taskDiv.classList.add("completed");
         }
        taskDiv.append(taskListItem);
        taskDiv.append(confirmButton);
        taskDiv.append(deleteButton);
        taskList.append(taskDiv);
    }
}



todoTask.addEventListener("click",addTodoComplete);
function addTodoComplete(e)
{
   const item=e.target;
    if(item.classList[0]==="delete-btn")
    {
        item.parentElement.classList.toggle("deleted");
        deleteLocalList(item.parentElement);
        item.parentElement.addEventListener("transitionend",e=>(item.parentElement.remove()));
    }
    if(item.classList[0]==="confirm-btn")
    {
        item.parentElement.classList.toggle("completed");
        addCompletedInLocal(item.parentElement);
    }
}

function saveLocalstorage(todoItem)
{
    var localTodos;
    if(localStorage.getItem("localTodos")=== null)
    {
        localTodos=[];
    } else{
        localTodos=JSON.parse(localStorage.getItem("localTodos"));
    }
    if(todoItem.length>=1)
    {
        localTodos.push(todoItem);
    }
    localStorage.setItem("localTodos",JSON.stringify(localTodos));
}
function localrender()
{
    var localTodos;
    if(localStorage.getItem("localTodos")=== null)
    {
        localTodos=[];
    } else{
        localTodos=JSON.parse(localStorage.getItem("localTodos"));
    }
    localTodos.forEach(createListContainerforLoad);
}

function deleteLocalList(task)
{
    var localTodos;
    if(localStorage.getItem("localTodos")=== null)
    {
        localTodos=[];
    } else{
        localTodos=JSON.parse(localStorage.getItem("localTodos"));
    }
   let listIndex= localTodos.indexOf(task.children[0].innerText);
   localTodos.splice(listIndex,1);
   localStorage.setItem("localTodos",JSON.stringify(localTodos));
   var complete;
   if(localStorage.getItem("complete")=== null)
   {
       complete=[];
   } else{
       complete=JSON.parse(localStorage.getItem("complete"));
   }
   let completeIndex=complete.indexOf(task.children[0].innerText);
   if(completeIndex>=0)
        {
            complete.splice(completeIndex,1);
            localStorage.setItem("complete",JSON.stringify(complete));
   }
}
function addCompletedInLocal(completeTask)
{
    var complete;
    if(localStorage.getItem("complete")=== null)
    {
        complete=[];
    } else{
        complete=JSON.parse(localStorage.getItem("complete"));
    }
    if(completeTask.classList.length==2){
        complete.push(completeTask.children[0].innerText);
    }
    else{
        if(complete.indexOf(completeTask.children[0].innerText)>=0)
        {
            complete.splice(complete.indexOf(completeTask.children[0].innerText),1);
        }
    }
        localStorage.setItem("complete",JSON.stringify(complete));
}
