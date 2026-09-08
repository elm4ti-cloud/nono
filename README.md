# nono

import json
import os

from transaccion import Transaccion


ARCHIVO = "transacciones.json"


def cargar_transacciones():

    if os.path.exists(ARCHIVO):

        try:
            with open(ARCHIVO, "r", encoding="utf-8") as archivo:
                datos = json.load(archivo)

            transacciones = []

            for dato in datos:
                transaccion = Transaccion.from_dict(dato)
                transacciones.append(transaccion)

            return transacciones

        except (json.JSONDecodeError, KeyError, ValueError):
            print("El archivo JSON contiene datos inválidos.")
            return []

    return []


def guardar_transacciones(transacciones):

    datos = []

    for transaccion in transacciones:
        datos.append(transaccion.to_dict())

    with open(ARCHIVO, "w", encoding="utf-8") as archivo:
        json.dump(
            datos,
            archivo,
            ensure_ascii=False,
            indent=4
        )


def agregar_casos_estudio(transacciones):

    ids_existentes = []

    for transaccion in transacciones:
        ids_existentes.append(transaccion.id)

    # Caso 1
    if 1 not in ids_existentes:
        transacciones.append(
            Transaccion(
                1,
                "Laura Gómez",
                3500000,
                2,
                "Colombia",
                False
            )
        )

    # Caso 2
    if 2 not in ids_existentes:
        transacciones.append(
            Transaccion(
                2,
                "Carlos Pérez",
                500000,
                14,
                "Colombia",
                True
            )
        )

    # Caso 3
    if 3 not in ids_existentes:
        transacciones.append(
            Transaccion(
                3,
                "Ana Torres",
                2500000,
                10,
                "Perú",
                True
            )
        )


def registrar_transaccion(transacciones):

    print("\n--- REGISTRAR TRANSACCIÓN ---")

    try:
        id = int(input("ID: "))

        titular = input("Titular: ")

        valor = float(input("Valor: "))

        hora = int(input("Hora (0-23): "))

        pais = input("País: ")

        dispositivo = input(
            "¿El dispositivo es conocido? (True/False): "
        )

        if dispositivo.lower() == "true":
            dispositivo_conocido = True

        elif dispositivo.lower() == "false":
            dispositivo_conocido = False

        else:
            print("Error: debe escribir True o False.")
            return

        # Verificar que el ID no esté repetido
        for transaccion in transacciones:
            if transaccion.id == id:
                print("Error: ya existe una transacción con ese ID.")
                return

        transaccion = Transaccion(
            id,
            titular,
            valor,
            hora,
            pais,
            dispositivo_conocido
        )

        transacciones.append(transaccion)

        guardar_transacciones(transacciones)

        print("\nTransacción registrada correctamente.")
        print("Puntaje:", transaccion.puntaje_riesgo)
        print("Clasificación:", transaccion.clasificacion)

    except ValueError as error:
        print("\nError:", error)


def listar_transacciones(transacciones):

    print("\n--- LISTA DE TRANSACCIONES ---")

    if len(transacciones) == 0:
        print("No hay transacciones registradas.")
        return

    for transaccion in transacciones:

        print("-----------------------------------")
        print("ID:", transaccion.id)
        print("Titular:", transaccion.titular)
        print("Valor:", transaccion.valor)
        print("Hora:", transaccion.hora)
        print("País:", transaccion.pais)
        print(
            "Dispositivo conocido:",
            transaccion.dispositivo_conocido
        )
        print("Puntaje:", transaccion.puntaje_riesgo)
        print("Clasificación:", transaccion.clasificacion)


def menu():

    # Cargar las transacciones existentes
    transacciones = cargar_transacciones()

    # Agregar los casos de estudio sin duplicarlos
    agregar_casos_estudio(transacciones)

    # Guardar los casos de estudio
    guardar_transacciones(transacciones)

    while True:

        print("\n==============================")
        print("   DETECTOR DE TRANSACCIONES")
        print("==============================")
        print("1. Registrar transacción")
        print("2. Listar transacciones")
        print("3. Salir")
        print("==============================")

        opcion = input("Seleccione una opción: ")

        if opcion == "1":

            registrar_transaccion(transacciones)

        elif opcion == "2":

            listar_transacciones(transacciones)

        elif opcion == "3":

            guardar_transacciones(transacciones)

            print("\nTransacciones guardadas correctamente.")
            print("Programa finalizado.")
            break

        else:

            print("\nOpción inválida. Intente nuevamente.")


if __name__ == "__main__":
    menu()

Con esto tienes:

- ✅ Los 3 casos de estudio incluidos.
- ✅ Los casos no se duplican al volver a ejecutar.
- ✅ Menú para registrar transacciones.
- ✅ Menú para listar transacciones.
- ✅ Opción para salir.
- ✅ Validación de "ID" repetido.
- ✅ Lectura desde "transacciones.json".
- ✅ Guardado mediante "to_dict()".
- ✅ Recuperación mediante "from_dict()".
- ✅ Cálculo automático de puntaje y clasificación.
