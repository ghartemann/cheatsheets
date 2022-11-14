## Snippets

### Call axios simple

```js
fetch() {
    axios.post('/log/fetch/' + this.$route.params.id).then((r) => {
        this.activityLogs = r.data;
    }).catch((error) => {
        this.$toastr.error("Erreur lors de la récupération de l'historique.");

        console.error(error);
    }).finally(() => {
        this.loading = false;
    });
}
```

### Call axios multiple

```js
axios.all([
    axios.get('/client/' + this.$route.params.id),
    axios.get('/client/' + this.$route.params.id + '/bien-vente'),
    axios.get('/client/' + this.$route.params.id + '/lot'),
    axios.get('/client/' + this.$route.params.id + '/flux'),
    axios.get('/pays/list')
]).then(axios.spread((
    clientRequest,
    bienVenteRequest,
    lotRequest,
    fluxRequest,
    paysRequest
) => {
    this.client = clientRequest.data;
    this.bienVente = bienVenteRequest.data;
    this.lot = lotRequest.data;
    this.flux = fluxRequest.data;
    this.listPays = paysRequest.data.__data;
})).catch((e) => {
    console.error(e);
}).finally(() => {
    this.loading = false;
});
```