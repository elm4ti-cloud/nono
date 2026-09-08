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

            return [Transaccion.from_dict(dato) for dato in datos]

        except (json.JSONDecodeError, KeyError, ValueError, TypeError):
            print("El archivo JSON contiene datos inválidos.")
            return []

    return []


def guardar_transacciones(transacciones):
    datos = [transaccion.to_dict() for transaccion in transacciones]

    with open(ARCHIVO, "w", encoding="utf-8") as archivo:
        json.dump(datos, archivo, ensure_ascii=False, indent=4)


def agregar_casos_estudio(transacciones):
    ids_existentes = {transaccion.id for transaccion in transacciones}

    casos_estudio = [
        Transaccion(
            1,
            "Laura Gómez",
            3500000,
            2,
            "Colombia",
            False
        ),
        Transaccion(
            2,
            "Carlos Pérez",
            500000,
            14,
            "Colombia",
            True
        ),
        Transaccion(
            3,
            "Ana Torres",
            2500000,
            10,
            "Perú",
            True
        )
    ]

    for transaccion in casos_estudio:
        if transaccion.id not in ids_existentes:
            transacciones.append(transaccion)


def registrar_transaccion(transacciones):
    print("\n--- REGISTRAR TRANSACCIÓN ---")

    try:
        identificador = int(input("ID: "))
        titular = input("Titular: ")
        valor = float(input("Valor: "))
        hora = int(input("Hora (0-23): "))
        pais = input("País: ")

        dispositivo = input(
            "¿El dispositivo es conocido? (True/False): "
        ).strip().lower()

        if dispositivo not in ("true", "false"):
            print("Error: debe escribir True o False.")
            return

        dispositivo_conocido = dispositivo == "true"

        if any(
            transaccion.id == identificador
            for transaccion in transacciones
        ):
            print("Error: ya existe una transacción con ese ID.")
            return

        transaccion = Transaccion(
            identificador,
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

    if not transacciones:
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
    transacciones = cargar_transacciones()

    agregar_casos_estudio(transacciones)
    guardar_transacciones(transacciones)

    while True:
        print("\n==============================")
        print("   DETECTOR DE TRANSACCIONES")
        print("==============================")
        print("1. Registrar transacción")
        print("2. Listar transacciones")
        print("3. Salir")
        print("==============================")

        opcion = input("Seleccione una opción: ").strip()

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
