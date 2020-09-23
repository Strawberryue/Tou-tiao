# Tou-tiao
my first Repository
    @GetMapping(path = {"/reglogin"})
    public String reglogin(Model model,@RequestParam(value = "next", required = false) String next) {
        model.addAttribute("next",next);
        return "login";
    }

    @PostMapping(value = "/login")
    public String login(Model model, String username, String password,
                        @RequestParam(value = "next", required = false) String next,
                        @RequestParam(value = "rememberme", defaultValue = "false") boolean rememberme,
                        HttpServletResponse response) {
        try {
            Map<String, Object> map = userService.login(username, password);
            if (map.containsKey("ticket")) {
                Cookie cookie = new Cookie("ticket", map.get("ticket").toString());
                cookie.setPath("/"); // 在同一应用服务器内共享cookie
                response.addCookie(cookie);
//                if (!StringUtils.isEmpty(next)) {
//                    return "redirect:" + next;
//                }
                return "redirect:/";
            } else {
                model.addAttribute("msg", map.get("msg"));
                return "login";
            }
        } catch (Exception e) {
            logger.error("登陆异常" + e.getMessage());
            return "login";
        }
    }
