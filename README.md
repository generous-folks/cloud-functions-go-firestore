# Go Firestore Rest API as cloud function

A simple CRUD implementation using Go with the Firestore AdminSDK aka the server side database client.

## Prerequisites

- Go > 1.11 installed
- A Google Account
- `gcloud` CLI installed globally
- A Firebase Project initiated with a Firestore Database
  > Alternatively you could not use Firebase at all but you would need to use a service account file and initialize the firestore client a slightly different way.

## Development

To test your function locally, you can add a `main` function with a http server to your code this way :

```go
func main() {
	// This example uses gorilla/mux as the router, whereas cloud functions are simple Http handlers
	router := mux.NewRouter()
	router.HandleFunc("/", YourFunction)

	srv := &http.Server{
		Handler:      router,
		Addr:         "0.0.0.0:8000",
		WriteTimeout: 10 * time.Seconds,
		ReadTimeout:  10 * time.Seconds,
	}

	log.Println("Running server on http://localhost:8000")
	log.Fatal(srv.ListenAndServe())
}
```

Although this is a way better solution than deploying your function to the cloud every changes, you might need to structure your code differently while keeping in mind you'll be deploying only one http handler function from a single file.

## Deploying your Cloud Function

- `gcloud auth login` Log into your google account
- `gcloud functions deploy YOUR_FUNCTION_NAME --runtime go111 --trigger-http --project YOUR_PROJECT_ID` Fill the uppercased placeholders
  > e.g. `gcloud functions deploy ArticleAPI --runtime go111 --trigger-http --project my-project`
