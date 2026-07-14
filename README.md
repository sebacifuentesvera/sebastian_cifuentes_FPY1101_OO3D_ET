# sebastian_cifuentes_FPY1101_OO3D_ET
planes = { 
    'F001': ['Plan Básico', 'mensual', 1, False, False, 'libre'], 
    'F002': ['Plan Full', 'mensual', 1, True, True, 'libre'], 
    'F003': ['Plan Estudiante', 'trimestral', 3, False, True, 'tarde'], 
    'F004': ['Plan Senior', 'trimestral', 3, True, False, 'mañana'], 
    'F005': ['Plan Anual Pro', 'anual', 12, True, True, 'libre'], 
    'F006': ['Plan Nocturno', 'mensual', 1, False, True, 'noche'] 
} 

inscripciones = {
'F001': [14990, 30],
'F002': [22990, 10],
'F003': [39990, 0],
'F004': [35990, 6],
'F005': [159990, 2],
'F006': [18990, 15]
}


def validar_codigo(codigo): 
    if codigo.strip() == "": 
        return False 
    if codigo.upper() in planes: 
        return False 
    return True 

def validar_nombre(nombre): 
    return nombre.strip() != "" 

def validar_tipo(tipo): 
    tipo = tipo.lower() 
    return tipo in ["mensual", "trimestral", "anual"]

def validar_duracion(duracion): 
    return duracion > 0 

def validar_sn(valor): 
    return valor.lower() in ["s", "n"]

def validar_horario(horario): 
    return horario.strip() != "" 

def validar_precio(precio): 
    return precio > 0 

def validar_cupos(cupos): 
    return cupos >= 0 

def cupos_tipo(tipo): 
    total = 0 
    for codigo in planes: 
        if planes[codigo][1].lower() == tipo.lower(): 
            total += inscripciones[codigo][1] 
    print(f"Total de cupos: {total}") 

def busqueda_precio(p_min, p_max): 
    lista = [] 
    for codigo in inscripciones: 
        precio = inscripciones[codigo][0] 
        cupos = inscripciones[codigo][1] 
        if p_min <= precio <= p_max and cupos > 0: 
            nombre = planes[codigo][0] 
            lista.append(f"{nombre}--{codigo}") 
            
    if len(lista) == 0: 
        print("No hay planes en ese rango de precios.") 
    else: 
        lista.sort() 
        for plan in lista: 
            print(plan) 

def actualizar_precio(codigo, nuevo_precio): 
    codigo = codigo.upper() 
    if codigo in inscripciones: 
        inscripciones[codigo][0] = nuevo_precio 
        return True 
    else: 
        return False 

def leer_opcion(): 
    while True: 
        print("\n========= MENÚ PRINCIPAL =========") 
        print("1. Cupos por tipo de plan") 
        print("2. Búsqueda de planes por rango de precio") 
        print("3. Actualizar precio de plan") 
        print("4. Agregar plan") 
        print("5. Eliminar plan") 
        print("6. Salir") 
        opcion = input("Seleccione una opción: ") 
        
        if opcion == "1": 
            tipo = input("Tipo de plan: ") 
            cupos_tipo(tipo) 
            
        elif opcion == "2": 
            while True: 
                try: 
                    minimo = int(input("Precio mínimo: ")) 
                    maximo = int(input("Precio máximo: ")) 
                    if 0 <= minimo <= maximo: 
                        break 
                    else: 
                        print("Valores incorrectos.") 
                except ValueError: 
                    print("Debe ingresar valores enteros") 
            busqueda_precio(minimo, maximo) 
            
        elif opcion == "3": 
            seguir = "s" 
            while seguir.lower() == "s": 
                codigo = input("Código: ").upper() 
                while True: 
                    try: 
                        precio = int(input("Nuevo precio: ")) 
                        if precio > 0: 
                            break 
                        else: 
                            print("Debe ser mayor que cero.") 
                    except ValueError: 
                        print("Debe ingresar un número entero.") 
                
                if actualizar_precio(codigo, precio): 
                    print("Precio actualizado") 
                else: 
                    print("El código no existe") 
                seguir = input("¿Desea actualizar otro precio (s/n)? ") 
                
        elif opcion == "4": 
            codigo = input("Código: ").upper() 
            nombre = input("Nombre: ") 
            tipo = input("Tipo: ").lower() 
            try: 
                duracion = int(input("Duración: ")) 
                precio = int(input("Precio: ")) 
                cupos = int(input("Cupos: ")) 
            except ValueError: 
                print("Datos numéricos incorrectos.") 
                continue 
                
            piscina = input("¿Piscina? (s/n): ") 
            clases = input("¿Clases? (s/n): ") 
            horario = input("Horario: ") 
            
            if not validar_codigo(codigo): 
                print("Código inválido") 
            elif not validar_nombre(nombre): 
                print("Nombre inválido") 
            elif not validar_tipo(tipo): 
                print("Tipo inválido") 
            elif not validar_duracion(duracion): 
                print("Duración inválida") 
            elif not validar_sn(piscina): 
                print("Dato piscina inválido") 
            elif not validar_sn(clases): 
                print("Dato clases inválido") 
            elif not validar_horario(horario): 
                print("Horario inválido") 
            elif not validar_precio(precio): 
                print("Precio inválido") 
            elif not validar_cupos(cupos): 
                print("Cupos inválidos") 
            else: 
                planes[codigo] = [ 
                    nombre, 
                    tipo, 
                    duracion, 
                    piscina.lower() == "s", 
                    clases.lower() == "s", 
                    horario 
                ] 
                inscripciones[codigo] = [precio, cupos] 
                print("Plan agregado correctamente.") 
                
        elif opcion == "5": 
            codigo = input("Código del plan: ").upper() 
            if codigo in planes: 
                del planes[codigo] 
                del inscripciones[codigo] 
                print("Plan eliminado.") 
            else: 
                print("El código no existe.") 
                
        elif opcion == "6": 
            print("Programa finalizado.") 
            break 
        else: 
            print("Debe seleccionar una opción válida") 
