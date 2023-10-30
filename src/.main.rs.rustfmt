use std::fs;

fn has_yellow(s: &[u8]) -> bool {
    (240..=250).contains(&s[0]) && (207..=221).contains(&s[1]) && (114..=130).contains(&s[2])
}

fn has_white(s: &[u8]) -> bool {
    230 <= s[0] && 230 <= s[1] && 230 <= s[2]
}

fn main() {
    let frames_path = std::env::args().nth(1).unwrap();

    for e in fs::read_dir(&frames_path).unwrap() {
        let path = e.unwrap().path();

        if !path.is_file() {
            continue;
        }

        let image = match image::open(&path) {
            Ok(i) => i,
            Err(e) => {
                eprintln!("{}", e);
                continue;
            }
        };

        let colors = dominant_color::get_colors(image.to_rgb8().as_raw().as_slice(), false);
        let has_target_color = colors.windows(3).any(|s| has_yellow(s) || has_white(s));

        if has_target_color {
            let mut path_t = path.clone();

            let filename = path_t.file_name().unwrap().to_os_string();
            path_t.pop();

            path_t.push("res");
            path_t.push(filename);

            fs::copy(&path, &path_t).unwrap();
            println!("copied {:?} to {:?}", path, path_t);
        }
    }
}
