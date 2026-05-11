<img width="869" height="691" alt="image" src="https://github.com/user-attachments/assets/6344f5c0-e894-476d-99af-9064901cc447" />
###tablas
##producto
<img width="817" height="501" alt="image" src="https://github.com/user-attachments/assets/ead88238-da4f-4caf-95b9-39c94d57bc7f" />
##categoria
<img width="758" height="211" alt="image" src="https://github.com/user-attachments/assets/78902dfa-f5ca-4861-9bbf-1b62c3562b2a" />
##proveedor
<img width="764" height="357" alt="image" src="https://github.com/user-attachments/assets/e85b6be6-d866-46a2-9cbe-14d2123c9e0f" />
##cliente
<img width="750" height="361" alt="image" src="https://github.com/user-attachments/assets/0452375a-ddb6-47de-8c16-133b5f02d436" />
##empleado
<img width="777" height="393" alt="image" src="https://github.com/user-attachments/assets/2fa35c25-beeb-4482-ac6e-6f2aeff36880" />
##venta
<img width="764" height="452" alt="image" src="https://github.com/user-attachments/assets/1bfbc19f-290a-477a-bdec-d1b453c902f5" />
##detalle venta
<img width="748" height="338" alt="image" src="https://github.com/user-attachments/assets/a6e4d794-084c-4fdd-9bc7-de8c25447c90" />
##compra
<img width="755" height="368" alt="image" src="https://github.com/user-attachments/assets/29ce1024-bb8f-43cb-8d24-95e7155a6ff4" />
##detalle compra
<img width="744" height="303" alt="image" src="https://github.com/user-attachments/assets/d7e7cce9-b51d-4fde-85fa-93224f4fe356" />
##inventario
<img width="767" height="357" alt="image" src="https://github.com/user-attachments/assets/a166177c-2475-44f9-b8ce-b6b74acc54f4" />



<div id="erd"></div>
<style>
#erd svg { max-width: 100%; }
</style>
<script type="module">
import mermaid from 'https://esm.sh/mermaid@11/dist/mermaid.esm.min.mjs';
const dark = matchMedia('(prefers-color-scheme: dark)').matches;
await document.fonts.ready;
mermaid.initialize({
  startOnLoad: false,
  theme: 'base',
  fontFamily: '"Anthropic Sans", sans-serif',
  themeVariables: {
    darkMode: dark,
    fontSize: '13px',
    fontFamily: '"Anthropic Sans", sans-serif',
    lineColor: dark ? '#9c9a92' : '#73726c',
    textColor: dark ? '#c2c0b6' : '#3d3d3a',
  },
});
const diagram = `erDiagram
  PRODUCTO ||--o{ DETALLE_VENTA : contiene
  PRODUCTO ||--o{ DETALLE_COMPRA : contiene
  PRODUCTO }o--|| CATEGORIA : pertenece
  PRODUCTO }o--|| PROVEEDOR : suministrado_por
  PRODUCTO ||--o{ INVENTARIO : registrado_en
  CLIENTE ||--o{ VENTA : realiza
  VENTA ||--|{ DETALLE_VENTA : incluye
  EMPLEADO ||--o{ VENTA : atiende
  PROVEEDOR ||--o{ COMPRA : surte
  COMPRA ||--|{ DETALLE_COMPRA : incluye
  EMPLEADO ||--o{ COMPRA : gestiona

  PRODUCTO {
    int id_producto PK
    string codigo_barras
    string nombre
    string descripcion
    int id_categoria FK
    int id_proveedor FK
    decimal precio_compra
    decimal precio_venta
    int stock_actual
    int stock_minimo
    string unidad_medida
  }
  CATEGORIA {
    int id_categoria PK
    string nombre
    string descripcion
  }
  PROVEEDOR {
    int id_proveedor PK
    string nombre
    string rfc
    string contacto
    string telefono
    string email
    string direccion
  }
  CLIENTE {
    int id_cliente PK
    string nombre
    string telefono
    string email
    string direccion
    string rfc
    date fecha_registro
  }
  VENTA {
    int id_venta PK
    int id_cliente FK
    int id_empleado FK
    datetime fecha
    decimal subtotal
    decimal descuento
    decimal iva
    decimal total
    string tipo_pago
    string estado
  }
  DETALLE_VENTA {
    int id_detalle PK
    int id_venta FK
    int id_producto FK
    int cantidad
    decimal precio_unitario
    decimal descuento
    decimal subtotal
  }
  COMPRA {
    int id_compra PK
    int id_proveedor FK
    int id_empleado FK
    datetime fecha
    decimal total
    string estado
    string folio_factura
  }
  DETALLE_COMPRA {
    int id_detalle PK
    int id_compra FK
    int id_producto FK
    int cantidad
    decimal costo_unitario
    decimal subtotal
  }
  INVENTARIO {
    int id_movimiento PK
    int id_producto FK
    datetime fecha
    string tipo_movimiento
    int cantidad
    int stock_resultante
    string motivo
  }
  EMPLEADO {
    int id_empleado PK
    string nombre
    string puesto
    string telefono
    string email
    string usuario
    string password_hash
    date fecha_ingreso
    boolean activo
  }
`;
const { svg } = await mermaid.render('erd-svg', diagram);
document.getElementById('erd').innerHTML = svg;
document.querySelectorAll('#erd svg.erDiagram .node').forEach(node => {
  const firstPath = node.querySelector('path[d]');
  if (!firstPath) return;
  const d = firstPath.getAttribute('d');
  const nums = d.match(/-?[\d.]+/g)?.map(Number);
  if (!nums || nums.length < 8) return;
  const xs = [nums[0], nums[2], nums[4], nums[6]];
  const ys = [nums[1], nums[3], nums[5], nums[7]];
  const x = Math.min(...xs), y = Math.min(...ys);
  const w = Math.max(...xs) - x, h = Math.max(...ys) - y;
  const rect = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
  rect.setAttribute('x', x); rect.setAttribute('y', y);
  rect.setAttribute('width', w); rect.setAttribute('height', h);
  rect.setAttribute('rx', '8');
  for (const a of ['fill', 'stroke', 'stroke-width', 'class', 'style']) {
    if (firstPath.hasAttribute(a)) rect.setAttribute(a, firstPath.getAttribute(a));
  }
  firstPath.replaceWith(rect);
});
document.querySelectorAll('#erd svg.erDiagram .row-rect-odd path, #erd svg.erDiagram .row-rect-even path').forEach(p => {
  p.setAttribute('stroke', 'none');
});
</script>
