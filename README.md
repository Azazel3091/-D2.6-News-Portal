# -D3.6-News-Portal Яковлев А. А. группа FPW-100
(рекомендую открыть файл "команды Django.txt" для удобного чтения.)
https://github.com/Azazel3091/-D3.6-News-Portal/blob/main/%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D1%8B%20Djando.txt

python manage.py shell
from newsportal.models import * 			#импорт модулей
u1 = User.objects.create_user(username='Andrew')	#создали пользователя Andrew
u2 = User.objects.create_user(username='Yuri')		#создали пользователя Yuri
Author.objects.create(authorUser=u1)			#создали объект модели Author для пользователя Andrew
Author.objects.create(authorUser=u2)			#создали объект модели Author для пользователя Yuri

Category.objects.create(name='Sport')			#создали 4 объекта категории в модель Category
Category.objects.create(name='Politics')		
Category.objects.create(name='Education')
Category.objects.create(name='Weather')

author = Author.objects.get(id=1)			#создали переменную для первого Автора
author2 = Author.objects.get(id=2)			#создали переменную для второго Автора

Post.objects.create(author=author, categoryType='AR', title='Заголовок', text='Текст статьи')				#создали первую статью
Post.objects.create(author=author, categoryType='AR', title='Заголовок второй статьи', text='Текст второй статьи')	#создали вторую статью
Post.objects.create(author=author, categoryType='NW', title='Заголовок новостей', text='Текст новостей')		#создали новости
Post.objects.create(author=author2, categoryType='AR', title='Заголовок статьи', text='Текст статьи')			#создали статью второго автора

Post.objects.get(id=1).postCategory.add(Category.objects.get(id=1))		#создали 2 категории 'Sport'и'Education' для первой статьи
Post.objects.get(id=1).postCategory.add(Category.objects.get(id=3)) 
Post.objects.get(id=2).postCategory.add(Category.objects.get(id=2)) 		#создали категорию 'Politics' для второй статьи
Post.objects.get(id=3).postCategory.add(Category.objects.get(id=4))		#создали категорию 'Weather' для новостей

Comment.objects.create(commentPost=Post.objects.get(id=1), commentUser=Author.objects.get(id=1).authorUser, text='Текст  комментария')				#создали комментарий к первой статье	
Comment.objects.create(commentPost=Post.objects.get(id=2), commentUser=Author.objects.get(id=1).authorUser, text='Текст  комментария второй статьи')		#создали комментарий к второй статье
Comment.objects.create(commentPost=Post.objects.get(id=3), commentUser=Author.objects.get(id=1).authorUser, text='Текст  комментария новостей')			#создали комментарий к новостям
Comment.objects.create(commentPost=Post.objects.get(id=3), commentUser=Author.objects.get(id=2).authorUser, text='Текст второго комментария новостей')		#создали комментарий к новостям от второго пользователя
Comment.objects.create(commentPost=Post.objects.get(id=4), commentUser=Author.objects.get(id=1).authorUser, text='Текст комментария к статье второго автора')	#создали комментарий к статье второго автора от первого пользователя

Comment.objects.get(id=1).like() 		#создали рейтинг комментариев
Comment.objects.get(id=2).like() 
Comment.objects.get(id=3).dislike() 
Comment.objects.get(id=4).like() 
Comment.objects.get(id=5).like() 

Post.objects.get(id=1).like() 		#создали рейтинг статей/новостей
Post.objects.get(id=2).like() 
Post.objects.get(id=3).dislike() 
Post.objects.get(id=4).like() 

a1 = Author.objects.get(id=1)		#создали переменные для авторов
a2 = Author.objects.get(id=2)

a1.update_rating()			#узнаем рейтинг первого автора
a1.ratingAuthor

a2.update_rating()			#узнаем рейтинг второго автора
a2.ratingAuthor

Author.objects.order_by('-ratingAuthor')[:1]	#узнаем лучшего автора

b = Author.objects.order_by('-ratingAuthor')[0]	#создаем переменную лучшего пользователя
b.authorUser, b.ratingAuthor 			#выводим username и рейтинг лучшего пользователя
(<User: Yuri>, 11)

Post.objects.order_by('-rating').values('dateCreation', 'author__authorUser__username', 'rating', 'title')[0]		#выводим дату добавления, username автора, рейтинг, заголовок лучшей статьи
Post.objects.order_by('-rating')[0].preview()										#выводим превью лучшей статьи

Post.objects.order_by('-rating')[0]							#узнаем лучшую статью
best_art = Post.objects.order_by('-rating')[0]						#создаем переменную лучшей статьи
best_art.comment_set.all().values('dateCreation', 'commentUser', 'rating', 'text')	#выводим все комментарии (дата, пользователь, рейтинг, текст) к лучшей статье.
