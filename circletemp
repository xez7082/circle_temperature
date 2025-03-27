<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Widget Température Jeedom</title>
    <style>
        .box_full {
            border-radius: 50%; /* Rendre la box circulaire */
            border: 3px solid #888888; /* Bordure par défaut gris neutre */
            background: #AAAAAA; /* Couleur de fond par défaut */
            width: 75px; /* Largeur du cercle */
            height: 75px; /* Hauteur du cercle */
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            position: relative;
        }

        .valeurtar {
            font-size: 20px; /* Ajuster la taille de la température */
            color: white;
            text-align: center;
        }

        .box_bottom {
            display: none; /* On n'affiche pas cette partie pour ce widget */
        }

        .cmdNametar#id# {
            text-align: center;
            mix-blend-mode: difference;
        }

        .unitetar {
            font-size: 12px;
            position: absolute;
            bottom: 50px;
         	
        }

        .decimaltar {
            font-size: 14px;
        }

        .cmdStats {
            display: none; /* Masquer les stats */
        }
.unitetar {
    font-size: 14px;
    position: absolute;
    top: 10px; /* Ajuster selon vos besoins */
    left: 50%;
    transform: translateX(-50%);
}

.box_full {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

    </style>
</head>
<body>
    <div class="cmd cmd-widget #history#" data-type="info" data-subtype="numeric" data-template="badge" data-cmd_id="#id#" data-cmd_uid="#uid#" data-version="#version#" data-eqLogic_id="#eqLogic_id#">
        <div class="box_full">
            <div style="text-align: center;">
                <div class="unitetar unite">#unite#</div>
<div>
    <span class="valeurtar value">#value#</span><span class="decimaltar"></span>
</div>
          </div>
        </div>
        <div class="box_bottom">
            <div class="title #hide_name#">
                <div class="cmdNametar#id# cmdName">nom#id#</div>
            </div>
        </div>
        <div class="cmdStats #hide_history#">
            <div class="col-xs-12 center-block">
                <span title='Min' class='tooltips'>#minHistoryValue#</span>|<span title='Moyenne' class='tooltips'>#averageHistoryValue#</span>|<span title='Max' class='tooltips'>#maxHistoryValue#</span> <i class="#tendance#"></i>
            </div>
        </div>

        <script>
            jeedom.cmd.update['#id#'] = function(_options) {
                var cmd = $('.cmd[data-cmd_id=#id#]');
                // Définition des paramètres
                var nom#id# = ('#nom#' != '#' + 'nom#') ? "#nom#" : "#name#"; // Remplacer le nom de la commande par celui de votre choix
                var couleurch#id# = ('#couleurch#' != '#' + 'couleurch#') ? "#couleurch#" : "#fc0303"; // Couleur chaude

                // Gamme de couleurs : blanc -> bleu -> vert -> orange -> rouge -> violet
                var couleurfr#id# = ('#couleurfr#' != '#' + 'couleurfr#') ? "#couleurfr#" : "#FFFFFF"; // Blanc au début

                var valmin#id# = parseInt('#minValue#'); // Température minimale
                var valmax#id# = parseInt('#maxValue#'); // Température maximale
                var valeur = parseFloat(_options.display_value).toFixed(0)
                var entier = valeur.split('.')[0];
				var decimal = valeur.split('.')[1];

                // Utilisation de textContent pour insérer du texte, pas du HTML
                cmd.find('.cmdNametar#id#').empty().text(nom#id#); // Remplacer append() par text()
                cmd.find('.decimaltar').empty().text(decimal);

                // Convertit une couleur hex en [r, g, b]
                function h2r(hex) {
                    var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
                    return result ? [
                        parseInt(result[1], 16),
                        parseInt(result[2], 16),
                        parseInt(result[3], 16)
                    ] : null;
                }

                // Convertit les valeurs RGB en HSL
                function rgb2hsl(r, g, b) {
                    r /= 255;
                    g /= 255;
                    b /= 255;
                    var max = Math.max(r, g, b);
                    var min = Math.min(r, g, b);
                    var h, s, l = (max + min) / 2;

                    if (max === min) {
                        h = s = 0; // achromatic
                    } else {
                        var d = max - min;
                        s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
                        switch (max) {
                            case r:
                                h = (g - b) / d + (g < b ? 6 : 0);
                                break;
                            case g:
                                h = (b - r) / d + 2;
                                break;
                            case b:
                                h = (r - g) / d + 4;
                                break;
                        }
                        h /= 6;
                    }

                    return [h * 360, s, l];
                }

                // Convertit les valeurs HSL en RGB
                function hsl2rgb(h, s, l) {
                    h /= 360;
                    var r, g, b;

                    if (s === 0) {
                        r = g = b = l; // achromatic
                    } else {
                        var q = l < 0.5 ? l * (1 + s) : (l + s) - (l * s);
                        var p = 2 * l - q;
                        r = hue2rgb(p, q, h + 1 / 3);
                        g = hue2rgb(p, q, h);
                        b = hue2rgb(p, q, h - 1 / 3);
                    }

                    return [Math.round(r * 255), Math.round(g * 255), Math.round(b * 255)];
                }

                // Aide pour calculer la couleur en fonction de l'angle de teinte
                function hue2rgb(p, q, t) {
                    if (t < 0) t += 1;
                    if (t > 1) t -= 1;
                    if (t < 1 / 6) return p + (q - p) * 6 * t;
                    if (t < 1 / 2) return q;
                    if (t < 2 / 3) return p + (q - p) * (2 / 3 - t) * 6;
                    return p;
                }

                // Fonction d'interpolation des couleurs
                function interpolateHSL(color1, color2, factor) {
                    if (arguments.length < 3) {
                        factor = 0.5;
                    }
                    var hsl1 = rgb2hsl(color1[0], color1[1], color1[2]);
                    var hsl2 = rgb2hsl(color2[0], color2[1], color2[2]);
                    for (var i = 0; i < 3; i++) {
                        hsl1[i] += factor * (hsl2[i] - hsl1[i]);
                    }
                    return hsl2rgb(hsl1[0], hsl1[1], hsl1[2]);
                }

                // Plages de températures et couleurs correspondantes
                var couleurs = [
                    { tempMin: -20, tempMax: -10, color: "#FFFFFF" }, // Blanc de -20 à -10
                    { tempMin: -10, tempMax: 0, color: "#0000FF" }, // Bleu de -10 à 0
                    { tempMin: 0.1, tempMax: 10, color: "#00FF00" }, // Vert de 0,1 à 10
                    { tempMin: 10.1, tempMax: 20, color: "#FFA500" }, // Orange de 10,1 à 20
                    { tempMin: 20.1, tempMax: 30, color: "#FF0000" }, // Rouge de 20,1 à 30
                    { tempMin: 30.1, tempMax: 50, color: "#8A2BE2" }  // Violet de 30,1 à 50
                ];

                // Déterminer la couleur correspondant à la température actuelle
                var color;
                for (var i = 0; i < couleurs.length; i++) {
                    if (valeur >= couleurs[i].tempMin && valeur <= couleurs[i].tempMax) {
                        color = h2r(couleurs[i].color);
                        break;
                    }
                }

                // Appliquer la couleur au fond
                cmd.find('.box_full').css('background-color', 'rgb(' + color + ')');

                // Créer une couleur beaucoup plus foncée pour la bordure
                var darkerColor = color.map(function(c) {
                    return Math.max(0, c - 120); // Assombrir chaque composant RGB beaucoup plus
                });

                // Appliquer la couleur foncée à la bordure
                cmd.find('.box_full').css('border', '3px solid rgb(' + darkerColor + ')');

                // Calculer l'intensité de la couleur pour ajuster la couleur du texte
                var brightness = 0.2126 * color[0] + 0.7152 * color[1] + 0.0722 * color[2];
                var textColor = brightness < 128 ? '#FFFFFF' : '#000000'; // Si sombre, texte blanc, sinon texte noir

                // Appliquer la couleur du texte
                cmd.find('.valeurtar').css('color', textColor);
                cmd.find('.unitetar').css('color', textColor);
                cmd.find('.decimaltar').css('color', textColor);
            }

            jeedom.cmd.update['#id#']({
                display_value: '#state#',
                valueDate: '#valueDate#',
                collectDate: '#collectDate#',
                alertLevel: '#alertLevel#'
            });
        </script>
    </div>
</body>
</html>

