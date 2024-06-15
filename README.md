В качестве результата задания подготовл файл, в котором написан список всех команд, запускаемых в Django shell

0. Импорт всех модулей

from news.models import *

1. Создать двух пользователей (с помощью метода User.objects.create_user('username')).

User1 = User.objects.create_user('Соловьев')   
User2 = User.objects.create_user('Крестов')

2. Создать два объекта модели Author, связанные с пользователями.

Author.objects.create(authorUser = User1) 
Author.objects.create(authorUser = User2)

3. Добавить 4 категории в модель Category.

Category.objects.create(categoryName = 'Спорт')
Category.objects.create(categoryName = 'Политика')
Category.objects.create(categoryName = 'Образование') 
Category.objects.create(categoryName = 'Культура')

4. Добавить 2 статьи и 1 новость.

Post.objects.create(author = authorUser, categoryType = 'AR', title = 'Рекорд', text = 'Очень длинный текст') 
Post.objects.create(author = authorUser, categoryType = 'AR', title = 'Магия', text = 'Коротенькая история')
Post.objects.create(author = authorUser, categoryType = 'NW', title = 'Свежие новости', text = 'Не законные гонки в центре города')

5. Присвоить им категории (как минимум в одной статье/новости должно быть не меньше 2 категорий).

Post.objects.get(id=1).postCategory.add(Category.objects.get(id=1)) 
Post.objects.get(id=1).postCategory.add(Category.objects.get(id=4)) 

6. Создать как минимум 4 комментария к разным объектам модели Post (в каждом объекте должен быть как минимум один комментарий).

Comment.objects.create(commentPost=Post.objects.get(id=1), commentUser=Author.objects.get(id=1).authorUser, text = 'Очень информативная статья')  
Comment.objects.create(commentPost=Post.objects.get(id=2), commentUser=Author.objects.get(id=1).authorUser, text = 'Ну пойдет')           
Comment.objects.create(commentPost=Post.objects.get(id=3), commentUser=Author.objects.get(id=2).authorUser, text = 'Что-то новенькое!')
Comment.objects.create(commentPost=Post.objects.get(id=1), commentUser=Author.objects.get(id=2).authorUser, text = 'Чушь собачья')


7. Применяя функции like() и dislike() к статьям/новостям и комментариям, скорректировать рейтинги этих объектов.

Comment.objects.get(id=1).like()
Post.objects.get(id=1).dislike()
Post.objects.get(id=3).like()
проверка рейтинга пользователя
Comment.objects.get(id=1).rating  
проверка рейтинга поста
Post.objects.get(id=1).rating


8. Обновить рейтинги пользователей.

u1 = Author.objects.get(id=1)
u1.update_rating()
u1.ratingAuthor
u2 = Author.objects.get(id=2)
u2.update_rating()
u2.ratingAuthor

9. Вывести username и рейтинг лучшего пользователя (применяя сортировку и возвращая поля первого объекта).

s = Author.objects.order_by('ratingAuthor')
 for i in s:    
...     i.ratingAuthor 
...     i.authorUser.username

10. Вывести дату добавления, username автора, рейтинг, заголовок и превью лучшей статьи, основываясь на лайках/дислайках к этой статье.

p = Post.objects.order_by('-rating')    
>>> for i in p[:1]:
...     i.dateCreation
...     i.author.authorUser
...     i.rating
...     i.title
...     i.preview()

11. Вывести все комментарии (дата, пользователь, рейтинг, текст) к этой статье.

Post.objects.all().order_by('-rating')[0].comment_set.values('dateCreation', 'commentUser', 'rating', 'text')
