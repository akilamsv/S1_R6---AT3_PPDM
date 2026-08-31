 # Expo Location 

 ## Introdução da biblioteca

 A biblioteca expo-location fornece APIs para ler informações de geolocalização dos dispositivos (Android e iOS) em aplicações React Native. Ela permite desde a captura da posição atual até o rastreamento em segundo plano e a conversão de endereços em coordenadas.

 ![InstalaçãoExpoLocation](instalacaoLOCATION.png)

 A biblioteca é instalada através do comando npm expo install expo-location, depois de instalada, sua importação no projeto é fundamental para seu uso.

 ## Configuração 
 A biblioteca pode ser configurada através de **puglins**. O plugin permite configurar diversas propriedades que não podem ser definidas em tempo de execução e que exigem a criação de um novo binário do aplicativo para entrarem em vigor. Se o seu aplicativo não utiliza CNG, você precisará configurar a biblioteca manualmente. 


 exemplo de puglins

 ```app.json
 {
  "expo": {
    "plugins": [
      [
        "expo-location",
        {
          "locationAlwaysAndWhenInUsePermission": "Allow $(PRODUCT_NAME) to use your location."
        }
      ]
    ]
  }
}
```

## Uso da Biblioteca
Caso utilize em emuladores de android ou simuladores, certifique-se que a localização esteja ligada durante o uso da biblioteca. 

Funcionamento dentro do código:

```javascript
import React, { useEffect, useState } from "react"
import { StyleSheet, Text, View } from "react-native"
import { SafeAreaView } from "react-native-safe-area-context"
import * as Location from 'expo-location';

export default function PosicaoGPSScreen() {
    const [location, setLocation] = useState(null);
    const [errorMsg, setErrorMsg] = useState(null);

    useEffect(() => {
        async function getCurrentLocation()  {
            const {status} = await Location.requestForegroundPermissionsAsync();
            if(status !== 'granted'){
                setErrorMsg("Permissão negada a localização!");
                return;
            }
            const tempLocation = await Location.getCurrentPositionAsync(); 
            setLocation(tempLocation);
        }
        getCurrentLocation();
    },[]);

    let text = 'Aguardando...';
    if(errorMsg){
        text = errorMsg;
    }else if (location) {
        text = JSON.stringify(location);
    }

    return (
        <SafeAreaView style={styles.safeArea}>
            <View style = {styles.container}>
                <View style = {styles.header}>
                    <Text style={styles.titleScreen}>Posição atual</Text>
                    <Text style={styles.paragraph}>{text}</Text>
                </View>
            </View>
        </SafeAreaView>
    );
```

![Funcionamento Location](funcionamentoLOCATION.png)

## Vantagens

- API Unificada: Código único em JavaScript que funciona perfeitamente tanto em iOS quanto em Android.
- Permissões Simplificadas: Métodos nativos prontos (requestForegroundPermissionsAsync) para solicitar autorização do usuário sem mexer em arquivos nativos complexos.
- Geocoding: Transforma um endereço de texto (ex: "Rua Augusta, São Paulo") em coordenadas de latitude e longitude.