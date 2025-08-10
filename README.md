# ENCODER_STM32_RJ_ELEKTRONIK_

Module STM32 Encoder – STM32F030F4P6
Ce module permet de lire la position d’un encodeur rotatif quadrature en utilisant un Timer du STM32 configuré en mode interface encodeur avec la bibliothèque HAL.

📌 Fonctionnalités
Lecture précise de la position en utilisant le matériel interne du STM32 (pas de polling lourd sur GPIO).

Gestion automatique du dépassement de compteur (overflow / underflow).

API simple pour :

Initialiser l’encodeur

Mettre à jour la position

Lire la position actuelle

Réinitialiser la position

⚙️ Matériel utilisé
Microcontrôleur : STM32F030F4P6

Encodeur rotatif (2 canaux A et B)

Timer : TIM3 (peut être adapté à un autre timer disponible)

📂 Fichiers
encoder.h → Définition de la structure et des fonctions

encoder.c → Implémentation du module

🛠 Configuration CubeMX
Activer TIM3 en mode Encoder Mode (TI1 and TI2)

Sélectionner deux GPIO connectés à CHA et CHB

Configurer :

Prescaler = 0

Counter Period = 65535

Générer le code (CubeIDE)

📜 Exemple d’utilisation
c
Copier
Modifier
#include "encoder.h"

Encoder_HandleTypeDef hEncoder;

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_TIM3_Init(); // Configuré en mode encodeur par CubeMX

    Encoder_Init(&hEncoder, &htim3);

    while (1) {
        Encoder_Update(&hEncoder);
        int32_t pos = Encoder_GetPosition(&hEncoder);
        printf("Position: %ld\r\n", pos);
        HAL_Delay(100);
    }
}
📖 Comment ça marche ?
Le Timer est configuré pour incrémenter ou décrémenter automatiquement en fonction des transitions des signaux A et B.

Encoder_Update() lit la valeur du compteur et calcule la différence depuis la dernière lecture.

En cas de dépassement (overflow ou underflow), le module ajuste automatiquement la valeur.

La position est stockée dans une variable 32 bits, permettant un comptage bien plus large que la taille du timer.

💡 Avantage : Ce code utilise zéro interruption manuelle pour compter les impulsions, c’est le hardware qui fait tout, ce qui réduit fortement la charge CPU.
