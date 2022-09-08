# Project setup

So you want to make a social app with Solid, right?

So how do you start?

They say Solid apps don't need backend. So let's just start with frontend...

Let's start with something you know well. [Create a React app](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app). With TypeScript and redux, apparently...

```shell
yarn create react-app ohn-solid-client --template redux-typescript
```

[Setup Prettier and linter](https://github.com/OpenHospitalityNetwork/ohn-solid/commit/d6fae3bad6d0832d701dbc82857cce46f45f6cae). So code is nice.

[Add workflow to deploy to github pages](https://github.com/OpenHospitalityNetwork/ohn-solid/commit/73d1221fa2e8b4fb0831e71140fccb4381a3fd78). Every commit to `main` branch will get deployed.

## Show a map

The central place of the app is a map which shows Offers from hosts.

[So let's make one. With Leaflet.](https://github.com/OpenHospitalityNetwork/ohn-solid/commit/a2555251023575657401e17df8ba91ea0c5fa084)

Still no Solid going on, right?

[Next: Start with Solid](solid-start.md)
