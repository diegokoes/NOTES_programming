# JakartaEE notes
## headers
request.[...]

## export

## cookie
> create

Cookie cookie = new Cookie("usuario", "nombreDeUsuario");

cookie.setMaxAge(7 * 24 * 60 * 60); 

response.addCookie(cookie);

cookie.setPath(request.getContextPath());

> read

Cookie[] cookies = request.getCookies();
if (cookies != null){
  for (Cookie c : cookies) {
      if (c.getName().equals("usuario")) {
          String nombreUsuario = c.getValue();
          // Realizar alguna acción con el valor de la cookie
          // out.println("Bienvenido de nuevo " + c.getValue());
      }
  }
}

> delete

Cookie cookie = new Cookie("usuario", "");
cookie.setMaxAge(0); // Se borra inmediatamente
cookie.setPath("/"); // Debe coincidir con el path de la cookie original
response.addCookie(cookie);


