Пройденный codelab https://developer.android.com/codelabs/car-app-library-fundamentals#0. Советую самостоятельно пройтись по пунктам из ссылки и посмотреть код на каждый пункт. 
Показывает базовые возможности написания приложения непосредственно под Automotive OS с использованием Android for Cars App Library, из категории PoI (Point of Interest)

Модуль :common содержит два модуля :data и :car-app-service. Первый представляем модель данных 
data class Place( val id: Int, val name: String, val description: String, val latitude: Double, val longitude: Double )
fun Place.toIntent(action: String): Intent { return Intent(action).apply { data = "geo:$latitude,$longitude".toUri() } } и репозиторий для доступа для других модулей. 

Второй модуль отвечает как раз за настройку работу приложения на бортовом компьютере. 
Там определен сервис создающий Session, сессия, в свою очередь отвечает за создание первого(стартового) экрана приложения. 
Дальнейшая навигация и взаимодействие настраивается в самом классе экрана.

Automotive имеет строгие требования к визуальной части, в связи с этим верстка строится на основе Templates, которые предоставляются самой библиотекой. 
Для определенных кейсов созданы специальные Templates. В примере используется PlaceListMapTemplate. 
Отрисовыает карту и дает возможность дополнить ее собственным списком точек интереса и отрисовку этих точек на карте.

Второй экран DetailScreen открывается при нажатии пользователя на один из элементов списка.
Содержит инфу о выбранной точке интереса и кнопку Navigate. Нажатие на кнопку вызывает настроенный action 
val navigateAction = Action.Builder() 
  .setTitle("Navigate") 
  .setIcon( CarIcon.Builder( IconCompat.createWithResource( carContext, R.drawable.baseline_navigation_24 ) )
  .build() ) 
  .setOnClickListener { carContext.startCarApp(place.toIntent(CarContext.ACTION_NAVIGATE)) } 
  .build()
После чего открывается системная карта и прокладывается маршрут по координатам из модели.


Особое внимание следует обратить на Manifest файлы модулей :car-app-service, :automotive и :app. Там есть комментарии от гугла. К пунктам без гугловских комментов добавил свои.
Еще раз обращаю внимание, что лучше самостоятельно пройтись по всем пунктам по ссылке https://developer.android.com/codelabs/car-app-library-fundamentals#0 для лучшего понимания.



