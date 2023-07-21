# How to update existing model instance in Django Rest Framework

## The Model


```
class Artist(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE, null=True)

    def __str__(self):
        return self.name
```

## The View


```
from rest_framework import generics

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
