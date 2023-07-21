# How to update existing model instance in Django Rest Framework

## The Model


```
class Artist(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE, null=True)
    name = models.CharField(max_length=128)
    slug = models.SlugField(unique=True)
    profile_image = models.FileField(null=True)
    cards = models.ManyToManyField(Card, null=True, related_name='artist_cards', blank=True)
    location = models.CharField(max_length=128, null=True)
    link = models.URLField(null=True)
    about = models.TextField(null=True)
    # f_song = models.ForeignKey(
    #     Song, null=True, on_delete=models.CASCADE, related_name="f_song"
    # )
    # feat_song = models.ForeignKey(
    #     FeaturedSong, null=True, on_delete=models.CASCADE
    # )
    songs = models.ManyToManyField(Song)
    email = models.EmailField(null=True)
    u_email = models.OneToOneField(Email, on_delete=models.CASCADE, blank=True, null=True)
    # artist_id = models.AutoField(default=1)

    # user = models.ForeignKey(User, on_delete=models.CASCADE, blank=True, null=True)

    def __str__(self):
        return self.name
```

## The View


```
class RetrieveArtistUserView(generics.RetrieveUpdateAPIView):
    queryset = Artist.objects.all()
    serializer_class = RetrieveArtistMusicSerializer
    permission_classes = [HasAPIKey | IsAuthenticated]
    lookup_field = 'u_email__email'
```

## The Route

```
urlpatterns = [
    path("", include(router.urls)),
    path("retrieve_artist_user/<u_email__email>", RetrieveArtistUserView.as_view(), name="retrieve_artist_user"),
    // YOUR OTHER ROUTES
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## The Query (Postman)

```
{
  "name": "Cheri Lyn"
}
```

## The Result

```
{
    "name": "Cheri Lyn",
    "other_things": []
}


