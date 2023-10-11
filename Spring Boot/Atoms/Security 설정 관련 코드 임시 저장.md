
```java
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        http
                .csrf((csrfConfig) ->
                        csrfConfig.disable()
                )
                .headers((headerConfig) ->
                        headerConfig.frameOptions(frameOptionsConfig ->
                                frameOptionsConfig.disable()
                        )
                )
                .authorizeHttpRequests((authorizeRequests) ->
                        authorizeRequests
                                .requestMatchers(PathRequest.toH2Console()).permitAll()
                                .requestMatchers("/", "/login/**").permitAll()
                                .requestMatchers("/posts/**", "/api/v1/posts/**").hasRole(Role.USER.name())
                                //.requestMatchers("/admins/**", "/api/v1/admins/**").hasRole(Role.ADMIN.name())
                                .anyRequest().authenticated()
                )
                .exceptionHandling((exceptionConfig) ->
                        exceptionConfig.authenticationEntryPoint(unauthorizedEntryPoint).accessDeniedHandler(accessDeniedHandler)
                )
                .formLogin((formLogin) ->
                        formLogin
                                .loginPage("/login/login")
                                .usernameParameter("username")
                                .passwordParameter("password")
                                .loginProcessingUrl("/login/login-proc")
                                .defaultSuccessUrl("/", true)
                )
                .logout((logoutConfig) ->
                        logoutConfig.logoutSuccessUrl("/")
                )
                .userDetailsService(myUserDetailsService);

        return http.build();
    }
```


```java
// 거의 가능성 보이는 코드 (였던것)
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    return http
            .csrf(AbstractHttpConfigurer::disable)
            .sessionManagement((sessionManagement) ->
                    sessionManagement.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            .authorizeHttpRequests((authorizeRequests) ->
                    authorizeRequests.anyRequest().permitAll()
            )
            .oauth2Login(oauth2Configurer -> oauth2Configurer
                    .loginPage("/login")
                    .successHandler(successHandler())
                    .userInfoEndpoint()
                    .userService(customOAuth2UserService))
            .build();
}

@Bean
public AuthenticationSuccessHandler successHandler() {
    return ((request, response, authentication) -> {
        DefaultOAuth2User defaultOAuth2User = (DefaultOAuth2User) authentication.getPrincipal();

        String id = defaultOAuth2User.getAttributes().get("id").toString();
        String body = """
                {"id":"%s"}
                """.formatted(id);

        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setCharacterEncoding(StandardCharsets.UTF_8.name());

        PrintWriter writer = response.getWriter();
        writer.println(body);
        writer.flush();
    });
}
```