import random

# Listas de productos, precios y stock
vproductos = ["Piedra Misteriosa", "Bromas Interdimensionales", "Taza del Triángulo", "Figura de Bill Cipher", 
               "Manual de Magia Oscura", "Camiseta de Mabel", "Cuerda de la Quinta Dimensión", 
               "Bebida de la Caverna", "Pociones de la Hechicera", "Cristal Dimensional"]

vprecios = [950, 150, 199, 2999, 899, 35, 519, 1889, 130, 1200]

vstock = [15, 30, 60, 7, 22, 28, 5, 6, 31, 4]

# Carrito de compras 
vcarrito = []

# Función para mostrar el inventario de productos
def vmostrar_inventario():
    print("--- INVENTARIO DE LA TIENDA DE LA CABAÑA DEL MISTERIO ---")
    for i in range(len(vproductos)):
        print(f"{vproductos[i]}: ${vprecios[i]} (Stock: {vstock[i]})")
    print("-------------------------------------------------------")

# Función para mostrar el carrito
def vmostrar_carrito():
    if len(vcarrito) == 0:
        print("El carrito está vacío.")
    else:
        print("--- TU CARRITO ---")
        vtotal = 0
        for vitem in vcarrito:
            print(f"{vitem['nombre']}: ${vitem['precio']} x{vitem['cantidad']}")
            vtotal += vitem['precio'] * vitem['cantidad']
        print(f"Total: ${vtotal}")
        print("--------------------")

# Función para agregar productos al carrito
def vagregar_al_carrito(vnombre_producto, vcantidad, vcarrito):
    if vnombre_producto in vproductos:
        vindice = vproductos.index(vnombre_producto)
        if vstock[vindice] >= vcantidad:
            # Crear una nueva lista para representar el carrito 
            vnuevo_carrito = vcarrito.copy()  # Copiar el carrito actual
            vitem_en_carrito = {"nombre": vnombre_producto, "precio": vprecios[vindice], "cantidad": vcantidad}
            vnuevo_carrito += [vitem_en_carrito]  # Agregar el nuevo item (articulo)
            vcarrito[:] = vnuevo_carrito  # Modificar el carrito original
            vstock[vindice] -= vcantidad
            print(f"{vcantidad} x {vnombre_producto} agregados al carrito.")
        else:
            print(f"No hay suficiente stock de {vnombre_producto}. Solo quedan {vstock[vindice]} disponibles.")
    else:
        print(f"El producto {vnombre_producto} no existe en la tienda.")

# Función para eliminar un producto del carrito
def veliminar_del_carrito(vnombre_producto, v_carrito):
    vitem_eliminado = False
    for i, vitem in enumerate(vcarrito):
        if vitem['nombre'] == vnombre_producto:
            vnuevo_carrito = vcarrito[:i] + vcarrito[i + 1:]  # Crear una nueva lista 
            vcarrito[:] = vnuevo_carrito  # Modificar el carrito original
            vindice = vproductos.index(vnombre_producto)
            vstock[vindice] += vitem['cantidad']
            print(f"{vnombre_producto} ha sido eliminado del carrito.")
            vitem_eliminado = True
            break
    if not vitem_eliminado:
        print(f"{vnombre_producto} no está en el carrito.")

# Función para aplicar un descuento aleatorio
def vaplicar_descuento():
    vdescuento = random.randint(5, 20)  # descuento aleatorio entre 5% y 20%
    print(f"¡Felicidades! Has recibido un descuento del {vdescuento}% en tu compra.")
    return vdescuento

# Función para realizar el pago
def vrealizar_pago(vcarrito):
    vtotal = sum(vitem['precio'] * vitem['cantidad'] for vitem in vcarrito)
    if len(vcarrito) == 0:
        print("El carrito está vacío, no puedes proceder con el pago.")
        return
    print("Resumen de tu compra:")
    vmostrar_carrito()
    vdescuento = vaplicar_descuento()
    vtotal_con_descuento = vtotal - (vtotal * vdescuento / 100)
    print(f"Total después del descuento: ${vtotal_con_descuento:.2f}")
    
    # Simulamos el proceso de pago, pidiendo el monto como cadena
    vpago = input("Introduce el monto para el pago: $")
    
    # Verificar que el monto introducido es solo un número
    vpago_valido = True
    for caracter in vpago:
        if caracter not in "0123456789.":
            vpago_valido = False
            break
    
    if vpago_valido:
        # Contamos el número de puntos (si hay más de uno, es inválido)
        if vpago.count('.') > 1:
            print("Por favor, ingresa un monto válido con un solo punto decimal.")
        else:
            vpago = float(vpago)
            if vpago >= vtotal_con_descuento:
                vcambio = vpago - vtotal_con_descuento
                print(f"Pago realizado con éxito. Tu cambio es: ${vcambio:.2f}")
                vcarrito[:] = []  # Vaciar el carrito después de la compra
            else:
                print("No tienes suficiente dinero para completar la compra.")
    else:
        print("Por favor, ingresa una cantidad válida.")

# Menú principal
def vmostrar_menu():
    print("--- Bienvenido a la Tienda de la Cabaña del Misterio ---")
    print("1. Ver inventario")
    print("2. Agregar al carrito")
    print("3. Eliminar del carrito")
    print("4. Ver carrito")
    print("5. Realizar pago")
    print("6. Revisar inventario al final del día")
    print("7. Salir")
    print("------------------------------------------------------")

# Función principal para ejecutar el programa
def vejecutar_programa():
    while True:
        vmostrar_menu()
        vopcion = input("Selecciona una opción (1-7): ")
        
        if vopcion == "1":
            vmostrar_inventario()
        elif vopcion == "2":
            vnombre_producto = input("¿Qué producto deseas agregar al carrito? ")
            if vnombre_producto in vproductos:
                vcantidad = input(f"¿Cuántos {vnombre_producto} deseas agregar? ")
                vcantidad_valida = True
                for caracter in vcantidad:
                    if caracter not in "0123456789":
                        vcantidad_valida = False
                        break
                if vcantidad_valida:
                    vagregar_al_carrito(vnombre_producto, int(vcantidad), vcarrito)
                else:
                    print("Por favor, ingresa una cantidad válida.")
            else:
                print("El producto no existe en la tienda.")
        elif vopcion == "3":
            vnombre_producto = input("¿Qué producto deseas eliminar del carrito? ")
            veliminar_del_carrito(vnombre_producto, vcarrito)
        elif vopcion == "4":
            vmostrar_carrito()
        elif vopcion == "5":
            vrealizar_pago(vcarrito)
        elif vopcion == "6":
            vmostrar_inventario()
        elif vopcion == "7":
            print("Gracias por visitar la Tienda de la Cabaña del Misterio. ¡Hasta luego!")
            break
        else:
            print("Opción no válida. Por favor, selecciona una opción entre 1 y 7.")

# Ejecutar el programa
if __name__ == "__main__":
    vejecutar_programa()
